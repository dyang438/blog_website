<script>
	import CodeBlock from "../../CodeBlock.svelte";
	import '../../../lib/styles/blog.css';

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
	
	// Add your code examples here
	let basic_borrow_example = `\`\`\`rust
fn main() {
    let mut data = vec![1, 2, 3, 4, 5];
    
    // This works - immutable borrow
    let first = &data[0];
    println!("First element: {}", first);
    
    // This would cause a compile error if uncommented:
    // data.push(6); // Cannot mutably borrow while immutably borrowed
}
\`\`\``;

	let ownership_example = `\`\`\`rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // s1 is moved to s2
    
    // println!("{}", s1); // This would error - s1 no longer valid
    println!("{}", s2); // This works
}
\`\`\``;

	let reference_example = `\`\`\`rust
fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope but doesn't drop the value (it's just a reference)

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1); // Pass reference, not ownership
    
    println!("The length of '{}' is {}.", s1, len); // s1 still valid
}
\`\`\``;

</script>

<svelte:head>
	<title>Opinionated Software and Rust's Borrow Checker</title>
	<meta name="description" content="Deep dive into Rust's borrow checker, ownership rules, and memory safety guarantees" />
</svelte:head>

<div class="all">
	<br /><br />

	<h1 class="title">
		Opinionated Software and Rust's Borrow Checker	
	</h1>

	<div>
		<h2>Introduction</h2>
		<p>The first thing I noticed learning rust, is that it seems to be made to annoy C++ devs into writing safe code.
		For the lazy dev, you're now "forced beyond the pale" to declare your variables mutable intentionally.</p>
	
		<p>This post is just a collage of such examples, and why undoing what feels like "tradition" gives people so much excitement.

		<em>Here is where I say "Blazing fast, memory safe."</em></p>

		<p>The aim of each section will be to disentangle what "new ideas" are useful in the compiler both in IR and once it gets to assembly, and which we can regard as an opinion based choice.</p>
		<h2>Warmup: The <span class="keyword">mut</span> keyword:</h2>

		<div class="code-block">
			<CodeBlock markdown={mut_example} />
		</div>

		<p>Intentionality is the key distinguisher on the qualitative value of a programming language, and it's clear in this case the language is right in helping the programmer think harder 
			about which variables need mutability. This has one benefit of managing side effects, but pushing code to have more const declarations has an effect I've written a snippet on <a href="/snippets/const_compiler">here</a>.</p>

		
		
	</div>
</div>

