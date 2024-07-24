<script>
	import CodeBlock from "../../CodeBlock.svelte";

	let isExpanded = false;

	function toggleExpand() {
		isExpanded = !isExpanded;
	}

	let deque_struct = `\`\`\`c
typedef struct deque_node 
{
	int payload;
	struct deque_node* next;
	struct deque_node* prev;
} 
deque_node;

typedef struct deque
{
	struct deque_node* first;
	struct deque_node* last;
}
deque;
\`\`\``;

	let deque_push = `\`\`\`c 
void deque_push_front(deque *input, int val)
{
    deque_node* new = malloc(sizeof(deque_node));
	new->payload = val;
	new->next = NULL;
	new->prev = NULL;
	input->length ++;

	if (!input->first || !input->last) {
		input->first = new;
		input->last = new;
	}

	input->first->prev = new;
	input->last->next = new;
	new->prev = input->last;
	new->next = input->first;
	input->first = new;
}
\`\`\``;

	let deque_pop = `\`\`\`c
int deque_pop_front (deque* input)
{
    // if the deque is empty, error
	if (!input || !input->first) {
		fprintf(stderr, "Popped empty deque");
		exit(-1);
	}

    // grab first item and update deque
	deque_node* popped = input->first;
	int ret = popped->payload;
	input->length--;

    // if the deque had one element return it
	if (input->length == 0) {
		free(popped);
		input->first = NULL;
		input->last = NULL;
		return ret;
	}

    // otherwise update pointer

	input->first->next->prev = input->last;
	input->last->next = popped->next;
	input->first = popped->next;

	free(popped);
	
	return ret;
}
\`\`\``;

	let pointer_bench = `\`\`\`c
deque* main_deque = init_deque();
for (int i = 0; i < TEST_SIZE; i++) 
{
	// push i onto the deque 1000 times, 
	deque_push_back(main_deque, i, PRINT);
}
for (int i = 0; i < MOVING_SIZE; i++)
{
	//pop the front and push the back
	int popped = deque_pop_front(main_deque);
	deque_push_back(main_deque, popped);
}
\`\`\``;

	let frag_bench = `\`\`\`c
for (int i = 0; i < TEST_SIZE; i++) 
{
    deque* main_deque = init_deque();
	deque* frag_deque = init_deque();

	for (int i = 0; i < TEST_SIZE; i++)
	{
		deque_push_front(main_deque, i);
		deque_push_front(frag_deque, i);
		deque_push_front(frag_deque, i);

		deque_push_back(main_deque, i);
		deque_push_front(frag_deque, i);
		deque_push_front(frag_deque, i);
	}
}

timer_start;
// Then iterate through the deque multiple times, making accessing node->payload
timer_end;

\`\`\``;

	let profile_used_payload = `\`\`\`X86 Assembly
.L16:
        mov     rcx, rdx               :rcx holds address of current node
        mov     rdx, QWORD PTR [rdx+8] :Accesses pointer to next node
        cmp     rdx, rax               :Checks if end of list
        jne     .L16                   :If not, continue
        mov     edx, DWORD PTR [rcx]
        mov     DWORD PTR [rsi], edx
\`\`\``;
</script>

<div class="all">
	<br /><br />

	<h1 class="title">
		Exploring the Deque, Maintaining Order in the Heap
	</h1>

	<div>
		<h2>Motivation:</h2>

		Cache locality is my favorite runtime factor to optimize for. It
		often leads to easy to visualize and neat "feeling" data
		structures, with the array being the prime example.<br /><br />

		The subject of spatial and reference locality is a constantly
		discussed optimization in high performance systems, as well as
		the effect on cache misses, which is good intuition to have for
		reading this. <br /><br />

		Here is a quick snippet on the basics of cache locality
		<a href={"../snippets/cache_locality"}>here</a>: <br /><br />

		Topics covered will be:
		<ul>
			<li>
				How <code>malloc()</code> practically allocates to
				the heap in C.
			</li>
			<li>
				Maximizing the deque's ability to take advantage
				of cache by understanding how to break it.
			</li>
			<li>
				Profiling the deque by running perf stat and
				looking at the X86 Assembly for a deque.
			</li>
		</ul>

		<h3>Current Deque Implementations:</h3>

		Here I'm going to be implementing a classic deque in C in style
		similar to the
		<code>c++ std::list</code>, a doubly linked-list:

		<h2>C Deque with Heap Allocation</h2>

		<div class="code-block">
			<CodeBlock markdown={deque_struct} />
		</div>

		Let us take a look at a pushing operation:

		<div class="code-block">
			<CodeBlock markdown={deque_push} />
		</div>

		Aside from all of the pointer manipulation, the large feature of
		this operation is the malloc'ing of a new node. Every node is
		created on the heap using a seperate call to malloc, meaning
		each allocation chooses the first available block large enough
		for a node. Here we correctly assume pushing consecutively to a
		deque multiple times would create a cache-friendly arrangement
		of nodes, as malloc will allocate nodes one after another on the
		heap.<br /><br />

		For throughness here is the popping operation for reference.
		<div class="code-block">
			<CodeBlock markdown={deque_pop} />
		</div>

		<h3>Lessons on Malloc</h3>

		Let us look at the following test bench:

		<div class="code-block">
			<CodeBlock markdown={pointer_bench} />
		</div>

		This test bench pushes<code>TEST_SIZE</code> elements onto the
		deque, and then pops the front and pushes the back
		<code>MOVING_SIZE</code>
		times. <br /><br />

		When we malloc each of the pushed nodes, where do we expect the
		nodes to be in memory? <br /><br />

		On the first passthough where we allocate a deque, we allocate
		the nodes in a contiguous block of memory. This is exactly
		expected as malloc finds the first free block that is large
		enough. <br /><br />

		Here are the pointer locations in the 10 nodes allocated,
		truncated to the last 4 digits. <br />
		<code
			>0x92c0 0x92e0 0x9300 0x9320 0x9340 0x9360 0x9380 0x93a0
			0x93c0 0x93e0</code
		><br /><br />

		When we pop the front and push the back, it ends up pushing the
		new node exactly where the last one was popped, resulting in the
		following pointer locations: <br />
		<code
			>0x92c0 0x92e0 0x9300 0x9320 0x9340 0x9360 0x9380 0x93a0
			0x93c0 0x93e0</code
		><br /><br />

		The important detail here is that as we pop memory, we free it.
		This allows the freed block to be placed back in list of free
		blocks and immediately reallocated. This is promising for deque
		usage in a vacuum when the only operations being used are
		popping and pushing to the front and back.

		<h3>Fragmenting A Deque</h3>

		To measure the effect of cache locality, we want to build a
		fragmented deque, meaning the deque nodes are spread
		irregularly, leading to cache rarely prefetching.<br /><br />

		We can fragment the deque by allocating a large block of memory
		between the push and pop operations. <br /><br />

		The following code block shows the test bench with the added
		fragmentation:

		<div class="code-block">
			<CodeBlock markdown={frag_bench} />
		</div>

		As these deque's are being malloc'd to the same heap, between
		two nodes in main_deque, the deque we are iterating through,
		there will be two trash nodes.<br /><br />

		The deque with fragmentation will have a higher cache miss rate,
		and thus will be slower than the deque without fragmentation.
		<br /><br />

		The cache block is fragmented by sizes of
		<code>2*sizeof(deque_node)</code>
		or 48 bytes. This causes deque_nodes to be greater than 64 (theoretically,
		72) bytes away from eachother, which is greater than the size of
		a cache-line.

		<h3>Why this matters</h3>

		To build intuition on why this would cause a dramatic slowdown,
		prefetching must be understood. Whenever a reference is accessed
		in DRAM, a surrounding "cache-line", normally of size 64 bytes,
		is pulled with it into the much quicker cache. This dramatically
		increases the speed of memory accesses when data we look at is
		already prefetched into cache. When our 24 byte struct is
		fetched on its own, as we iterate through the deque we don't
		prefetch any other nodes when there is 48 bytes separating it
		from the nearest neighbor. This causes a much higher amount of
		nodes to not be prefetched to cache.

		<h2>How big is the effect in practice? Breaking the deque.</h2>

		When we run this bench with and without fragmentation, we can
		see the effect that cache locality has on the deque.<br /><br
		/>(Average samples from 5 runs)

		<ul>
			<li>
				Without Fragmentation, Forward Traversal: 0.275s
			</li>
			<li>
				Without Fragmentation, Reverse Traversal: 0.258s
			</li>
			<li>With Fragmentation Forward Traversal: 1.260s</li>
			<li>With Fragmentation Reverse Traversal: 1.147s</li>
		</ul>

		Fragmentation results in a slowdown of around 4-5x in this case.
		When I tested this with my windows machine, I got a slowdown of
		9-10x! This resulted just from using another deque being pushed
		to at the same time.<br /><br />

		<h2>Profiling the Code</h2>

		When viewing the X86 Assembly for this bench at -O2 optimization
		for gcc, we get this result for the traversal (the benched
		part).

		<div class="code-block">
			<CodeBlock markdown={profile_used_payload} />
		</div>

		As we can see, there are no heavy instructions here, the
		slowness being incurred through this loop is caused by how far
		the mv instruction has to go (often all the way to DRAM) in
		order to grab the next node.

		<h3>How Does This Factor Into Best Coding Practices?</h3>

		The deque's circular, double-sided nature makes it a suitable
		alternative to both linked-lists and stacks. Obviously many
		software systems rely on its efficient implementation.<br /><br
		/>

		While working with a deque, it is important to consider where
		memory is allocated. If a deque is being created or changed
		while other threads are malloc'ing to the same heap, it can be
		useful to rebuild it atomically (in relation to other heap
		allocations) to regain the cache speedup.
		<br /><br />

		You could also consider forking a new process if the deque
		calculations are not reliant on the other heap collection's
		states. Process isolation means the child will have a different
		heap than the parent, but forking itself is a process on the
		order of 1ms, which is often much too long for certain higher
		performance applications. Threads share the heap though, so
		spawning a thread is a faster, but ineffectual process for this
		use case.<br /><br />

		More likely, for performance critical applications the array and
		arraylist are your best friends. A lot of times, even using a
		hashmap can prefetch cache more efficiently (as in an
		open-addressing system), since the hashmap will be contiguous in
		memory. <br /><br />

		This involves reliance on a good hashing function for efficiency
		though, and having a high load factor can cause dramatic
		slowdown depending on the implementation of the hashtable.
		<br /><br />

		I typically avoid using deques unless I am optimizing for how
		easy they are to work with, which often justifies their use.
		That's probably why I have a soft spot for them, and made my
		first post about making them as efficient as possible.
		<br /><br /><br />

		<button class="clickable-div" on:click={toggleExpand}>
			Testing Environment:
		</button>

		{#if isExpanded}
			<ul class="testing-environment">
				<li>WSL2 running OpenSUSE-Tumbleweed</li>
				<li>16GB Ram</li>
				<li>AMD X86 Processor</li>
				<li>gcc 14.1.1</li>
			</ul>
			Thanks for reading!
		{:else}
			<br /><br />
		{/if}
	</div>
</div>

<style>
	.all {
		font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
	}
	h1 {
		width: 100%;
		font-weight: bold;
	}
	h2 {
		width: 100%;
		line-height: 150%;
		font-weight: bold;
		margin-bottom: 0;
	}
	.code-block {
		padding: 5px;
		border-radius: 5px;
		overflow-x: auto;
	}
</style>
