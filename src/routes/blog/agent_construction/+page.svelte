<script>
	import '../../../lib/styles/blog.css';
	import HiddenPost from '../../HiddenPost.svelte';

	const isHidden = true;
	const completionPercentage = 0;
</script>

<svelte:head>
	<title>Agent Construction</title>
	<meta name="description" content="" />
</svelte:head>

{#if isHidden}
	<HiddenPost title="Agent Construction" completionPercentage={completionPercentage} />
{:else}
<div class="all">
	<br /><br />

	<div class="title-section">
		<h1 class="title">Agent Construction</h1>
	</div>

	<div>
	</div>
</div>
{/if}

<style>
	.title-section {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1rem;
	}
</style>
