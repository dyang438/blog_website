<script>
	// import CodeBlock from "../../CodeBlock.svelte";
	import '../../../lib/styles/blog.css';

</script>

<svelte:head>
	<title>Pushing a Paper in a Month: My Winter at Reticular AI (YC F'24)</title>
	<meta name="description" content="My experience contributing to ML research and using Lightning AI's compute ecosystem." />
</svelte:head>

<div class="all">
	<br /><br />

	<h1 class="title">
		Scaling a Model in a Month: My Winter at Reticular AI (YC F'24)
	</h1>

	<div>
		<h2>Introduction</h2>
		<p>This winter I had the unique opportunity to work at Reticular AI, a Y Combinator F'24 startup focused on AI for biology.

		<h2>The Research Challenge</h2>
		<p>When I joined, the team had already found their interesting problem. We were going to extend this paper: <a href="https://github.com/ElanaPearl/InterPLM" target="_blank" rel="noopener noreferrer">InterPLM</a></p>
		<p>This paper had found that we could extract interpretable features from protein language models with SAEs, but had interpreted only the smallest ESM model by meta research.</p>
		<p>We talked to Elana (who had written the paper). She wasn't focused on more than a POC for her paper, but it wasn't possible to scale too much more because of a couple of reasons. The main one that sticks with me is that at Stanford the GPUs are all limited on memory, and moreso that she couldn't choose which GPU to use within the Stanford server. This meant that she didn't know how much memory she could load on at once to maximize training speed.</p>
		<p>Working at a new YC with all of these AWS compute credits to burn then was cleanly a huge opportunity to extend that paper.</p>

		<h2>What we were Scaling</h2>
		<p>Looking back, to explain my contribution it's not even necesssary to deep dive SAE architecture at all, so for simplicty sake I'll just drop a link to the <a href="https://transformer-circuits.pub/2023/monosemantic-features/index.html" target="_blank" rel="noopener noreferrer">seminal Anthropic paper here</a> and abstract the models as a matmul.</p>
		<p>SAEs are used for interpreting transformer models though, with the activations of a NN layer as we run inference being the input to the SAE model.</p>
		<p>Our goal was to scale the InterPLM model from interpreting layers from ESM-8M to ESM-3B. Our end-goal wasn't fully decided at first, but we chose 3B because of its good performance and that it unlocked the ability to link the model with ESM-Fold, which unlocked "structure prediction." This sounds super cool to me but I didn't necessarily think about those implications myself.</p>
		<p>In terms of providing some quantitative heuristics, we had a parameter increase from 8 million to 3 billion (375x increase). This sounds like a complete redesign is in order, but SAEs tran on layers, which only had an increase in embedding dimension (our input space) from 320 to 2560 (8x). This means we still had to speed up our training by a lot, but only by about 100x, and our memory usage wouldn't be overly taxed by scaling the model (once again about 100x).</p>
		<p>InterPLM had gotten its results with 10 million proteins from UniProt as the inputs to embeddings, and we ended up using hte same amount for our final SAEs. To get our data (embeddings) from these sequences, we had to run inference using ESM-3B and pull the activations and store them. It just about ended up that 10 million embeddings was about 10TB(!) of data going through the model.</p>
		<p>In terms of time for training, we wanted it to be as fast as possible, as we didn't just want to train one SAE for each layer and be done, but we wanted to iterate on the dictionary learning framework, as well as hyperparameter tune as much as possible.</p>
		<p>Skipping forward to the result, we were able to train an SAE in 12 hours on a 8xL40S instance, and getting it that fast was a huge bonus for pushing the paper out.</p>

		<h2>Getting the Input Materials</h2>
		<p>The first goal was as follows: get the embeddings for 3B</p>
		<p>This ended up requiring our first innovation, which was asynchronous job scheduling.</p>
		<p>We were using Lighting AI as our compute platform, a service which essentially abstracts all the headaches of AWS Sagemaker into a browser click and run service.</p>
		<p>By running our inference tasks on several cheap AWS GPU ec2 instances, Lightning provided a place where it took only a couple of days to grab all of the embeddings we needed, so as far as code changes, we didn't have any groundbreaking ones for this process.</p>
		<p>After we had started doing the dataloader optimization, we started using S3 buckets to store our embeddings.</p>

		<h2>Distributing Training</h2>
		<p>I'm going to handwave some of this section, and just say that <a href="https://github.com/Lightning-AI/pytorch-lightning" target="_blank" rel="noopener noreferrer">Pytorch Lightning</a> was a mature and very easy to use library for our case. It is an open source project that abstracts distributed training from the user. This was super useful as we could just run our models on as many GPUs as we needed.</p>
		<p class="note">This gave us a clean 8x bump because we could go from using 1xL40S machines to 8x without sacrificing compute time.</p>
		<p>I basically just went through and rewrote the entire training step with the CEO to fit the Pytorch Lightning framework, while making sure our code was still learning the same way. The way we verified this was by tracking our loss on weights and biases.</p>

		<h2>Optimizing Dataset, Dataloader -- Using Streaming</h2>
		<p>This is the most interesting contribution I've ever made to any project, and getting this to work was somewhat of a stretch goal but ended up being a clean 10x difference in compute cost and speed after we processed our data more.</p>
		<p>I motivated the problem by stating our input data was, in totality on the magnitude of 10TB for 10M protein sequences of ESM-3B embeddings. To feed all of this into our model, the dataloader would need to be incredibly good. This is doubly true in the context of our model. The SAE is an incredibly simple model, where we merely run one forward step, one filtering step, and one backprop on a single matmul. This means our training speed was completely hindered by dataloader speed, which is exactly the problem that every AI company is running in to these days. Our data ingestion model was fine up until being fed into the dataloader, where we needed to increase performance.</p>
		<p>The main library we leveraged to complete this was <a href="https://github.com/Lightning-AI/litData" target="_blank" rel="noopener noreferrer">litdata</a>, whose main feature is to take datasets and optimize the input data as flattened binaries.</p>
		<h3>Avoiding working Backwards</h3>
		<p>We ended up running into a couple limitations along the way though, the first set had the same solution. </p>
		<p>Firstly, when processing our data with the .optimize() function, speed would curb hard before we could hit 1TB of optimized data. This happened because we just had too many binary stores for the library's memory management to handle.</p>
		<p>Secondly, I had optimized each batch to be individually loadable. This made it too slow to rebatch from our shuffled dataloader's activations. </p>
			<p class="example-blurb">In our case, we had 2048 activations as our batch dimension (being fed into GPU during one training step). To fill out one batch for one run in the GPU, we would need to load singular activations one by one and then recombine these activations into one 2048 sized batch.</p>
		<p>InterPLM and us had been cheating on the dataloader by prebatching our data by randomizing our activations during storage. This means that we weren't shuffling during training time, but instead during pre-processing.</p>
		<p>Having used the function now, we figured out a way to keep that pre-processing stage shuffling manuver while provably keeping our model random. This wasn't that difficult but we ended up getting concerned and having a couple of discussions about it, but we ended up being convinced.</p>
		<p>After testing we batched with 2048 activations, and if we had 8 gpus we would just put the batch size into Pytorch Lightning as 8 * 2048.</p>
			<p class="note">This makes it so that a lot more training data is required to let the loss converge all of the way, as the amount of data used increases for the same number of training steps.</p>

			

	</div>
</div>

