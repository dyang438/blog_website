<script>
	import CodeBlock from "../../CodeBlock.svelte";

	let const_example = `\`\`\`c
// Without const - compiler must assume variable can change
int sum_array_mutable(int* arr, int size) {
    int sum = 0;  // Mutable variable
    for(int i = 0; i < size; i++) {
        sum += arr[i];
    }
    return sum;
}

// With const - compiler can optimize more aggressively  
int sum_array_const(const int* arr, int size) {
    int sum = 0;  // Could be const after initialization
    for(int i = 0; i < size; i++) {
        sum += arr[i];  // arr[i] guaranteed not to change
    }
    return sum;
}
\`\`\``;

</script>

<svelte:head>
	<title>Const in the Compiler - Performance Benefits</title>
	<meta name="description" content="How const declarations help compilers optimize code and improve performance" />
</svelte:head>

<div class="all">
	<br /><br />

	<h1 class="title">
		Const in the Compiler
	</h1>

	<div>
		<h2>Introduction</h2>
		<p>The <code>const</code> keyword isn't just documentation - it's a promise to the compiler that enables powerful optimizations. When you declare something as const, you're giving the compiler permission to assume that value won't change, opening up optimization opportunities.</p>

		<h2>Basic Example</h2>

		<div class="code-block">
			<CodeBlock markdown={const_example} />
		</div>

		<p>In the const version, the compiler knows that <code>arr[i]</code> values can't be modified through this pointer, allowing for optimizations like:</p>
		
		<ul>
			<li><strong>Register allocation</strong> - Values can be cached in registers longer</li>
			<li><strong>Loop optimizations</strong> - Compiler can unroll or vectorize more aggressively</li>
			<li><strong>Memory aliasing</strong> - Less worry about pointer aliasing issues</li>
		</ul>

		<h2>Performance Impact</h2>
		
		<p>The performance benefits vary but can include:</p>
		<ul>
			<li>Reduced memory access overhead</li>
			<li>Better instruction scheduling</li> 
			<li>More aggressive inlining decisions</li>
			<li>Elimination of redundant loads</li>
		</ul>

		<p>While modern compilers are quite smart, <code>const</code> removes ambiguity and allows the optimizer to work more effectively.</p>

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

	code {
		background-color: rgba(0, 0, 0, 0.1);
		padding: 2px 4px;
		border-radius: 3px;
		font-family: 'Fira Mono', monospace;
		font-size: 0.9em;
	}
</style>