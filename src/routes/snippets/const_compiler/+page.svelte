<script>
	import CodeBlock from "../../CodeBlock.svelte";

	let boundary_example = `\`\`\`cpp
int mutable_factor = 10;
const int const_factor = 10;

__attribute__((noinline)) void side_effect() {
    mutable_factor = 20;
}

int scale_mutable(int x) {
    int a = x * mutable_factor;
    side_effect();               // mutable_factor might have changed
    int b = x * mutable_factor; // compiler must reload
    return a + b;
}

int scale_const(int x) {
    int a = x * const_factor;
    side_effect();               // const_factor cannot have changed
    int b = x * const_factor;   // compiler skips the reload
    return a + b;
}
\`\`\``;

	let boundary_assembly = `\`\`\`x86asm
scale_mutable(int):
        mov     eax, DWORD PTR mutable_factor[rip]   ; first load
        call    side_effect()
        imul    eax, edi
        imul    edi, DWORD PTR mutable_factor[rip]   ; forced reload after call
        add     eax, edi
        ret
scale_const(int):
        call    side_effect()
        lea     eax, [rdi+rdi*4]                     ; no reload needed
        sal     eax, 2                               ; = 20x (both multiplications folded)
        ret
main:
        ...
        mov     esi, 100                             ; scale_const(5) computed at compile time
\`\`\``;

	let mut_example = `\`\`\`rust
fn main() {
	let normal_var:i32 = 20;
	let mut var_assigned_mut:i32 = 20;

	// this won't compile
	normal_var += 12

	// clearly this will
	var_assigned_mut += 12
}
\`\`\``;

	let const_example = `\`\`\`c
// mutable global - compiler must reload from memory every call
int factor = 10;

int scale(int x) {
    return x * factor;
}

// const global - compiler bakes the value in directly
const int FACTOR = 10;

int scale_const(int x) {
    return x * FACTOR;
}
\`\`\``;

	let assembly_example = `\`\`\`x86asm
scale(int):
        mov     eax, DWORD PTR factor[rip]   ; load factor from memory
        imul    eax, edi
        ret
scale_const(int):
        lea     eax, [rdi+rdi*4]             ; 5*x using address hardware
        add     eax, eax                     ; double it -> 10*x, no memory access
        ret
factor:
        .long   10
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
		<p>The compiler's job is to eliminate redundant work. To do that, it needs to know what can't change. For local variables and simple expressions, it figures this out on its own. The place it genuinely needs your help is across function call boundaries — any call to an external function could theoretically modify a global, so the compiler has to assume the worst and reload.</p>
		<p><code>const</code> is the promise that removes that assumption. Not because it enforces anything at runtime, but because it gives the compiler information it couldn't derive itself. That's when you see it actually matter in the output.</p>

		<h2>Basic Example</h2>

		<div class="code-block">
			<CodeBlock markdown={const_example} />
		</div>

		<p>With a mutable global, the compiler has to assume something else could modify <code>factor</code> between calls, so it emits a memory load every time. With <code>const</code>, it knows the value at compile time and never touches memory. The assembly makes this obvious:</p>

		<div class="code-block">
			<CodeBlock markdown={assembly_example} />
		</div>

		<p>There's a bonus: the const version doesn't even use <code>imul</code>. The compiler replaces multiplication by 10 with a <code>lea</code> (computes 5x using address calculation hardware) followed by an <code>add</code> to double it — strength reduction that's faster than a multiply.</p>

		<h2>Across Function Call Boundaries</h2>

		<div class="code-block">
			<CodeBlock markdown={boundary_example} />
		</div>

		<div class="code-block">
			<CodeBlock markdown={boundary_assembly} />
		</div>

		<p>After <code>side_effect()</code>, the mutable version reloads <code>mutable_factor</code> from memory — it has to, since the function could have changed it. The const version doesn't bother. In <code>main</code>, <code>scale_const(5)</code> compiles down to <code>mov esi, 100</code> — the entire computation was done at compile time.</p>

		<h2>The <span class="keyword">mut</span> Keyword in Rust</h2>
		<p>Rust makes this tradeoff explicit by design. Every variable is immutable by default — you have to opt into mutability with mut. I originally interpreted it while learning Rust as a choice made to annoy C++ devs into writing safe code. For the lazy dev, you're now "forced beyond the pale" to declare your variables mutable intentionally.</p>

		<div class="code-block">
			<CodeBlock markdown={mut_example} />
		</div>

		<p>Intentionality is the key distinguisher on the qualitative value of a programming language, and it's clear in this case the language is right in helping the programmer think harder about which variables need mutability. This has one benefit of managing side effects, but the deeper payoff is that pushing code toward more const declarations gives the compiler exactly the information it needs to do its job.</p>

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