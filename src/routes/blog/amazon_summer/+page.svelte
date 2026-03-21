<script>
	import CodeBlock from "../../CodeBlock.svelte";
	import '../../../lib/styles/blog.css';
	import HiddenPost from '../../HiddenPost.svelte';

	const isHidden = true;
	const completionPercentage = 25;
</script>

<svelte:head>
	<title>My Summer at Amazon</title>
	<meta name="description" content="Reflections on my summer internship experience at Amazon" />
</svelte:head>

{#if isHidden}
	<HiddenPost
		title="My Summer at Amazon"
		completionPercentage={completionPercentage}
	/>
{:else}
	<div class="all">
	<br /><br />

	<h1 class="title">
		My Summer at Amazon
	</h1>

	<div>
		<h2>Introduction</h2>
		<p>This summer I had the opportunity to intern at Amazon. This post covers my experience, what I learned, and reflections on working at one of the world's largest tech companies.</p>

		<h2>The Role</h2>
		<p>I worked as a Software Development Engineer Intern on [Team Name]. My project involved [brief description of what you worked on].</p>

		<h2>What I Learned</h2>
		<ul>
			<li>Technical skills and technologies used</li>
			<li>Working in a large-scale distributed systems environment</li>
			<li>Amazon's leadership principles in practice</li>
			<li>Code review processes and engineering standards</li>
		</ul>

		<h2>Challenges and Growth</h2>
		<p>Every internship comes with its challenges. Here are some of the key areas where I grew:</p>

		<p><strong>Technical Challenges:</strong> Working with large codebases, understanding existing systems, and implementing solutions that scale.</p>

		<p><strong>Communication:</strong> Learning to effectively communicate technical concepts and progress to team members and stakeholders.</p>

		<h2>The Amazon Culture</h2>
		<p>Amazon's 16 Leadership Principles weren't just posted on walls - they were actively used in decision making and daily work. Some that stood out to me:</p>

		<ul>
			<li><strong>Customer Obsession:</strong> Every decision started with the customer impact</li>
			<li><strong>Ownership:</strong> Taking end-to-end responsibility for projects</li>
			<li><strong>Dive Deep:</strong> Understanding systems at a detailed level</li>
		</ul>

		<h2>Key Takeaways</h2>
		<p>My summer at Amazon taught me valuable lessons about working in a fast-paced, large-scale technology environment. The experience reinforced my passion for software engineering and gave me insights into building systems that serve millions of customers.</p>

		<p><em>This post captures some initial thoughts - I plan to expand on specific technical learnings and projects in future posts.</em></p>

		<br /><br />
	</div>
</div>
{/if}

