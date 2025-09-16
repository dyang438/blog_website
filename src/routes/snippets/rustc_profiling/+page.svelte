<script>
	import CodeBlock from "../../CodeBlock.svelte";

	let setup_example = `\`\`\`bash
# Clone the Rust repository
git clone https://github.com/rust-lang/rust.git
cd rust

# Configure for profiling (enable debug symbols)
./configure --enable-debug --enable-profiler

# Build rustc with profiling enabled
./x.py build --stage 1
\`\`\``;

	let profiling_commands = `\`\`\`bash
# Profile rustc compilation with perf
perf record -g ./build/x86_64-unknown-linux-gnu/stage1/bin/rustc hello.rs

# Generate flame graph from perf data
perf script | stackcollapse-perf.pl | flamegraph.pl > rustc-profile.svg

# Profile memory usage with valgrind
valgrind --tool=massif ./build/x86_64-unknown-linux-gnu/stage1/bin/rustc hello.rs
\`\`\``;

	let time_passes = `\`\`\`bash
# Use rustc's built-in timing information
rustc -Z time-passes hello.rs

# More detailed timing with unstable flags
rustc -Z time-passes -Z time-llvm-passes hello.rs
\`\`\``;

</script>

<svelte:head>
	<title>Profiling rustc - Following the Dev Guide</title>
	<meta name="description" content="My experience following the rustc dev guide to profile Rust compiler performance" />
</svelte:head>

<div class="all">
	<br /><br />

	<h1 class="title">
		Profiling rustc: Following the Dev Guide
	</h1>

	<div>
		<h2>Introduction</h2>
		<p>This snippet documents my experience following the <a href="https://rustc-dev-guide.rust-lang.org/profiling.html" target="_blank" rel="noopener noreferrer">rustc profiling guide</a>. 
		The goal is to understand where the Rust compiler spends its time and identify potential performance bottlenecks.</p>

		<p>This is more of a learning exercise than a comprehensive analysis - just documenting the process and interesting findings along the way.</p>

		<h2>Initial Setup</h2>
		
		<p>First step is getting a debug build of rustc with profiling symbols:</p>

		<div class="code-block">
			<CodeBlock markdown={setup_example} />
		</div>

		<h2>Basic Profiling Commands</h2>

		<p>The guide suggests several approaches for profiling rustc compilation:</p>

		<div class="code-block">
			<CodeBlock markdown={profiling_commands} />
		</div>

		<h2>Built-in Timing</h2>

		<p>Rustc has some built-in profiling flags that are easier to use than external tools:</p>

		<div class="code-block">
			<CodeBlock markdown={time_passes} />
		</div>

		<h2>What I'm Looking For</h2>

		<p>Key questions I want to answer:</p>
		<ul>
			<li>Where does rustc spend most of its compilation time?</li>
			<li>How does compile time scale with code size/complexity?</li>
			<li>What are the memory allocation patterns?</li>
			<li>How much time is spent in LLVM vs rustc passes?</li>
		</ul>

		<h2>Progress Notes</h2>
		
		<p><em>This section will be updated as I work through the profiling process...</em></p>

		<p><strong>TODO:</strong></p>
		<ul>
			<li>Set up profiling environment</li>
			<li>Create test cases of varying complexity</li>
			<li>Run initial profiling sessions</li>
			<li>Analyze flame graphs and timing data</li>
			<li>Document interesting findings</li>
		</ul>

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

	a {
		color: var(--color-theme-1);
		text-decoration: underline;
	}

	a:hover {
		text-decoration: none;
	}

	em {
		font-style: italic;
		color: rgba(0, 0, 0, 0.7);
	}
</style>