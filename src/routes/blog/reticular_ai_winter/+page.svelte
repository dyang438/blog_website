<script>
	import CodeBlock from "../../CodeBlock.svelte";

</script>

<svelte:head>
	<title>Pushing a Paper in a Month: My Winter at Reticular AI (YC F'24)</title>
	<meta name="description" content="My experience contributing to AI research and publishing at a Y Combinator startup" />
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
		<p>Our goal was to scale the InterPLM model from interpreting layers from ESM-8M to ESM-3B. This wasn't fully decided at first, but it unlocked the ability to link the model with ESM-Fold, which unlocked "structure prediction." This sounds super cool to me but I didn't necessarily have a chance to see those implications myself.</p>
		<p>In terms of providing some quantitative heuristics, we had a parameter increase from 8 million to 3 billion (375x increase). This is huge overhead, but luckily each layer only had an increase in embedding dimension (our input space) from 320 to 2560 (8x) though. This means we still had to speed up our training by a lot, but our memory usage wouldn't be overly taxed by scaling the model by 375x.</p>
		<p>In terms of time for training, we wanted it to be as fast as possible, as we didn't just want to train one SAE for each layer and be done, but we wanted to iterate on the dictionary learning framework, as well as hyperparameter tune as much as possible.</p>
		<p>Skipping forward to the result, we were able to train an SAE in 12 hours on a 8xL40S instance, and getting it that fast was a huge bonus for pushing the paper out.</p>

		<h2>Getting the Input Materials</h2>
		<p>The first goal was as follows: get the embeddings for 3B</p>
		<p>This ended up requiring our first innovation, which was asynchronous job scheduling.</p>
		<p>We were using Lighting AI as our compute platform, a service which essentially abstracts all the headaches of AWS Sagemaker into a browser click and run service.</p>
		<p>By running our inference tasks on several cheap AWS GPU ec2 instances, Lightning provided a place where it took only a couple of days to grab all of the embeddings we needed, so as far as code changes, we didn't have any groundbreaking ones for this process.</p>
		<p>After we had started doing the dataloader optimization, we started using S3 buckets to store our embeddings.</p>
		<ul>
			<li>Speed up the dataloader be equipped to load larger data </li>
			<li></li>
			<li></li>
			<li></li>
		</ul>

		<h2>Distributing Training</h2>
		<p>I'm going to handwave some of this section, and just say that Pytorch Lightning is incredibly well done. It is an open source project that allows distributed training abstracted away from the user. This was super useful as we didn't know the number of GPUs that would be optimal to run our models on. </p>
		<p>This gave us a clean 8x bump because we could go from using 1xL40S machines to 8x with some extra compute cost.</p>
		<p>I basically just went through and rewrote the training step with the CEO, and we made sure our code was still learning the same way. The way we verified this was by tracking our loss on weights and biases.</p>

		<h2>Optimizing Dataset, Dataloader -- Using Streaming</h2>
		<br /><br />
	</div>
</div>

<style>
	.all {
		font-family: var(--font-body);
		max-width: 800px;
		margin: 0 auto;
		padding: 2rem;
	}

	h1 {
		font-weight: bold;
		margin-bottom: 2rem;
	}

	h2 {
		font-weight: bold;
		margin: 2rem 0 1rem 0;
		color: var(--color-theme-1);
	}

	h3 {
		font-weight: 600;
		margin: 1.5rem 0 0.5rem 0;
		color: var(--color-theme-1);
	}

	ul {
		margin: 1rem 0;
		padding-left: 2rem;
	}

	li {
		margin: 0.5rem 0;
	}

	strong {
		color: var(--color-theme-1);
		font-weight: 600;
	}

	em {
		font-style: italic;
		color: rgba(0, 0, 0, 0.7);
	}
</style>