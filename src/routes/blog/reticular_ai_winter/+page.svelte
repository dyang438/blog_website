<script>
	import '../../../lib/styles/blog.css';

	const completionPercentage = 90;
</script>

<svelte:head>
	<title>Scaling a Model in &lt;2 months: Engineering at Reticular AI (YC F'24)</title>
	<meta name="description" content="How I owned scaling an interpretable ML pipeline at a YC startup, cutting training time 10x and unlocking structure prediction." />
</svelte:head>

<div class="all">
	<br /><br />

	<div class="title-section">
		<h1 class="title">Scaling a Model in &lt;2 months: Engineering at Reticular AI (YC F'24)</h1>
		{#if completionPercentage < 100}
			<div class="completion-indicator">
				<div class="completion-percentage">{completionPercentage}% Complete</div>
			</div>
		{/if}
	</div>

	<div>
		<h2>Introduction</h2>
		<p>In Winter 2025 I had the unique opportunity to work at Reticular AI, a Y Combinator F'24 startup focused on AI for biology.</p>
		<p>We published <a href="https://arxiv.org/abs/2503.08764" target="_blank" rel="noopener noreferrer">Towards Interpretable Protein Structure Prediction with Sparse Autoencoders</a> at ICLR workshops and as of July 2025 we were highlighted in Anthropic's highly impactful Transformer Circuits Thread <a href="https://transformer-circuits.pub/2025/july-update/index.html" target="_blank" rel="noopener noreferrer">July Updates</a>.</p>
		<p>My role: I owned scaling our model. Keeping compute costs/SAE roughly the same, my contribution was scaling the model 10x and cutting training time 10x.</p>
		<p>For my efforts I was second-author with the two founders but we were all working tirelessly around the clock on different parts of the paper for the better part of two months.</p>

		<h2>What the Scaling Unlocked</h2>
		<p>When I joined, the research questions were completely hidden behind the golden door of "a scaled model." We didn't know what embeddings we were scaling to, nor what performance we'd see in any given metric. We didn't even have scaling laws to work off of. That's the beauty of working in research — "this" had never been done.</p>
		<p>The scaled model had two contributions: size and speed.</p>
		<p>Size gave us our headline. We realized we were extending interpretability to structure prediction — something nobody saw coming until I proved we could train SAEs efficiently on ESM-3B (the backbone of ESMFold). From there we could steer structure predictions like the paper touts.</p>
		<p>We didn't see crazy results from our initial scaled SAEs — it's not as if gold fell from the embedding layers and we just picked it up and bundled it. It was a matter of having more vectors to tease out possible interesting research from.</p>
		<p>Speed gave us room to actually do research. Broader hyperparameter sweeps and enough runs to experiment with different SAE architectures altogether. That's what led to the Matryoshka finding — structure prediction performance recoverable with only 8 to 32 active latents. Fewer than you'd expect for something as complex as a 3D fold.</p>
		<p>Neither result was on the roadmap when I started. Both fell out of simply having the scale to look.</p>

		<h2>My Challenge</h2>
		<p>When I joined, the team had already found their interesting problem. We were going to extend this paper: <a href="https://github.com/ElanaPearl/InterPLM" target="_blank" rel="noopener noreferrer">InterPLM</a></p>
		<p>This paper had found that we could mechanistically interpret features from protein language model layer embeddings with SAEs, but had interpreted only the smallest open-source ESM-8M model.</p>
		<p class="note">We spoke with Elana (author of InterPLM) on why she wasn't able to scale any more aggressively. At Stanford the types of GPUs at the time (2024) were all gaming GPUs limited on memory (think RTX 4070 12GB). On any given day she couldn't guarantee usage of the same GPU to use within the Stanford server, affecting batch size and model size considerations.</p>
		<p>Working at a company with all of these new YC-offer compute credits to burn was a huge opportunity to extend this work.</p>
		<p>I had a good fit because I was (at that point) newly armed with systems and ML knowledge to scale an in-house model that the two non-CS founders would have likely outsourced. In fact, before I came on, Reticular was considering paying another startup 150k+ on training this model. We ended up saving that cost by having me own developing the entire in-house training pipeline, a very special opportunity that I am proud to say I made the most of.</p>

		<h2>What we were Scaling</h2>
		<p>To explain my contribution it's not even necesssary to deep dive SAE architecture too much, so for simplicity's sake I'll just drop a link to the <a href="https://transformer-circuits.pub/2023/monosemantic-features/index.html" target="_blank" rel="noopener noreferrer">seminal Anthropic blog post here</a> and abstract the models as I see fit for blog post level detail.</p>
		<p>SAEs are used for interpreting transformer models though, with the activations of a NN layer as we run inference being the input to the SAE model.</p>
		<p>Our goal was to scale the InterPLM model to interpreting layers from models larger than ESM-8M. Our end-goal wasn't fully decided at first, but we chose 3B because of its good performance and that it unlocked the ability to link the model with ESM-Fold, which unlocked "structure prediction."</p>
		<p>In terms of providing some quantitative heuristics, we had a parameter increase from 8 million to 3 billion (375x increase). This sounds like a complete redesign is in order, but SAEs train on layers, which only had an increase in embedding dimension (our input space) from 320 to 2560 (8x). This means we still had to speed up our training by a lot, but only by about 100x, and our memory usage wouldn't be overly taxed by scaling the model (once again about 100x).</p>
		<p>InterPLM had gotten its results with 10 million proteins from UniProt as the inputs to embeddings, and we ended up using hte same amount for our final SAEs. To get our data (embeddings) from these sequences, we had to run inference using ESM-3B and pull the activations and store them. It just about ended up that 10 million embeddings was about 10TB(!) of data going through the model.</p>
		<p>In terms of time for training, we wanted it to be as fast as possible, as we didn't just want to train one SAE for each layer and be done, but we wanted to iterate on the dictionary learning framework, as well as hyperparameter tune as much as possible.</p>
		<p>Skipping forward to the result, we were able to train an SAE in 12 hours on a 8xL40S instance, and getting it that fast was a huge bonus for pushing the paper out.</p>

		<h2>Getting the Input Materials</h2>
		<p>The first goal was as follows: get the embeddings for ESM-3B.</p>
		<p>This ended up requiring our first process improvement, which was asynchronous job scheduling.</p>
		<p>We were using Lighting AI as our compute platform, a useful service which abstracts some of the headaches of AWS Sagemaker into a browser click and run items.</p>
		<p>By running our inference tasks on several cheap AWS GPU ec2 instances, Lightning provided a place where it took only a couple of days to grab all of the embeddings we needed.</p>
		<p>After we had started doing huge data like 3B, necessitating dataloader optimization, we started using S3 buckets to store our embeddings, but we started by storing in filesystems, which promptly broke at scale.</p>

		<h2>Distributing Training</h2>
		<p>I'm going to handwave some of this section, and just say that <a href="https://github.com/Lightning-AI/pytorch-lightning" target="_blank" rel="noopener noreferrer">Pytorch Lightning</a> was a mature and very easy to use library for our case. It is an open source project that abstracts distributed training from the user. This was super useful as we could just run our models on as many GPUs as we needed.</p>
		<p class="note">This gave us nearly a clean 8x bump because we could go from using 1xGPU machines to 8x.</p>
		<p>I went through and rewrote the entire training step to fit the Pytorch Lightning framework, while making sure our code was still learning the same way. The way we verified this was by tracking our loss on weights and biases.</p>

		<h2>Optimizing Dataset, Dataloader -- Using Streaming</h2>

		<h3>Decision-Making</h3>
		<p>At this point we had 3 weeks to finalize our SAEs we would analyze deeply for submission. Each SAE could take upwards of a week to run on 5 epochs even with distributed training. We had so many hyper parameters to tune and work with that we decided 3 iterations was suboptimal and would severely hinder our ability to get good results.</p>

		<h3>Engineering at Scale</h3>
		<p>This is the most interesting contribution I've ever made to any project, and getting this to work was somewhat of a stretch goal but ended up being a clean 10x difference in speed after we successfully processed our data this way.</p>
		<p>I motivated the problem by stating our input data was, in totality on the magnitude of 10TB for 10M protein sequences of ESM-3B embeddings. To feed all of this into our model, the dataloader would need to be incredibly good. This is doubly true in the context of our model. The SAE is an incredibly simple model, where we merely run one forward step, one filtering step, and one backprop on a single matmul. This means our training speed was completely hindered by dataloader speed.</p>
		<p>By profiling, we found our data ingestion model was fine up until being fed into the dataloader, where we needed to increase performance.</p>
		<p>The main library we leveraged to complete this was <a href="https://github.com/Lightning-AI/litData" target="_blank" rel="noopener noreferrer">litdata</a>, whose main feature is to take datasets and optimize the input data as flattened binaries.</p>

		<h3>Avoiding Working Backwards</h3>
		<p>We ended up running into a couple limitations along the way though.</p>
		<p>Firstly, when processing our data with the .optimize() function, speed would curb hard before we could hit 1TB of optimized data. This happened because we just had too many binary stores for the library's memory management to handle.</p>
		<p>Secondly, I had optimized each batch to be individually loadable. This made it too slow to rebatch from our shuffled dataloader's activations.</p>
		<p class="example-blurb"><strong>Example:</strong> In our case, we had 2048 activations as our batch dimension (being fed into GPU during one training step). To fill out one batch for one run in the GPU, we would need to load singular activations one by one and then recombine these activations into one 2048 sized batch.</p>
		<p>InterPLM and us had been prebatching our data by randomizing the order of our activations during storage. This means that we weren't shuffling during training time, but instead during pre-processing.</p>
		<p>Having used the function now, we figured out a way to keep that pre-processing stage shuffling manuver while keeping our model's training random.</p>
		<p>After testing we found batching by 2048 activations to give us enough gradient updates while keeping each batch efficiently large, which remained true up to 8 GPUs where we would just put the batch size into Pytorch Lightning as 8 * 2048.</p>
		<p>This subsequently allowed us to train our week long models in less than a day, a very ergonomic amount of time for quick iteration speed for hyperparameter tuning.</p>
	</div>
</div>

<style>
	.title-section {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1rem;
	}

	.completion-indicator {
		display: flex;
		justify-content: center;
	}

	.completion-percentage {
		background-color: #e1f5fe;
		color: #0277bd;
		border: 1px solid #81d4fa;
		border-radius: 8px;
		padding: 0.5rem 1rem;
		font-size: 0.9rem;
		font-weight: 600;
		white-space: nowrap;
	}

</style>
