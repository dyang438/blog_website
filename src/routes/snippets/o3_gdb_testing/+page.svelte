<script>
	import CodeBlock from "../../CodeBlock.svelte";

</script>

<svelte:head>
	<title>Testing O3 Compiler Level for GDB Debugging</title>
	<meta name="description" content="Exploring how aggressive compiler optimizations affect debugging experience with GDB" />
</svelte:head>

<div class="all">
	<br /><br />

	<h1 class="title">
		Testing O3 Compiler Level for GDB Debugging
	</h1>

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