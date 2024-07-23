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
        mov     edx, DWORD PTR [rcx]   :Accesses payload
        mov     DWORD PTR [rsi], edx   :Stores payload
\`\`\``;

</script>

<div class="all">
<br><br>

    <h1 class="title">Exploring the Deque, Feasability of Cache Locality in the Heap</h1>


    <div> 
        <h2>Motivation:</h2>

        Cache locality is my favorite runtime factor to optimize for. It started from seeing vectors outpace linked-lists in a demonstration during my operating systems class.
        
        There are many writings already on the subject of spatial and reference locality, as well as the effect on cache misses, which is good intuition to have.

        This post focuses on how <code>malloc()</code> practically allocates memory and how fragile a deque's ability to take advantage of cache is. 

        <h3>Current Deque Implementations:</h3>

        Here I'm going to be implementing a classic deque in C:

        <h2>C Deque with Heap Allocation</h2>

        <div class="code-block">
            <CodeBlock markdown={deque_struct} />
        </div>


        Let us take a look at a pushing operation:

        <div class="code-block">
            <CodeBlock markdown={deque_push} />
        </div>

        Aside from all of the pointer manipulation, the large feature of this operation is the malloc'ing of a new node. This means that every node is created on the heap using a relatively costly call to malloc.
        Here intuition suggests pushing consecutively to a deque multiple times would create a locality-friendly arrangement of nodes, and we can assume malloc will allocate nodes one after another. This does happen to be the case.<br><br>

        Let's also take a look at a popping operation:
        <div class="code-block">
            <CodeBlock markdown={deque_pop} />
        </div>

        Malloc alone dictates where the pushed node allocates, as the deque only cares about references to the node from its neighbors (and whether it's first or last in the deque). <br> <br>

        <h2>Lessons on Malloc</h2>

        Let us look at the following test bench:
        
         <div class="code-block">
            <CodeBlock markdown={pointer_bench} />
        </div>

        This test bench pushes <code>TEST_SIZE</code> elements onto the deque, and then pops the front and pushes the back <code>MOVING_SIZE</code> times. <br><br>

        When we malloc each of the pushed nodes, where do we expect the nodes to be in memory? <br><br>

        On the first passthough where we allocate a deque, we allocate the nodes in a contiguous block of memory. This is exactly expected as malloc finds the first free block that is large enough. <br><br>

        Here are the pointer locations in the 10 nodes allocated, truncated to the last 4 digits. <br>
        <code>0x92c0	0x92e0	0x9300	0x9320	0x9340	0x9360	0x9380	0x93a0	0x93c0	0x93e0</code><br><br>

        When we pop the front and push the back, it ends up pushing the new node exactly where the last one was popped, resulting in the following pointer locations: <br>
        <code>0x92c0	0x92e0	0x9300	0x9320	0x9340	0x9360	0x9380	0x93a0	0x93c0	0x93e0</code><br><br>  
        
        The important detail here is that as we pop memory, we free it. This allows the freed block to be placed back in list of free blocks and immediately reallocated. 

        <h2>Fragmenting A Deque</h2>

        To measure the effect of cache locality, we want to build a fragmented deque, meaning the deque nodes are spread irregularly, leading to cache rarely prefetching.<br><br>
        
        We can fragment the deque by allocating a large block of memory between the push and pop operations. <br><br>

        The following code block shows the test bench with the added fragmentation:

        <div class="code-block">
            <CodeBlock markdown={frag_bench} />
        </div>

        The cache block is fragmented by sizes of <code>2*sizeof(deque_node)</code> or 48 bytes. This causes deque_nodes to be greater than 64 (theoretically, 72) bytes away from eachother, which is the size of a cache-line. <br><br>
        I did an investigation in the form of a snippet LINK HERE: to measure whether the prefetched cache can be used on partially fetched structs. <br><br>

        When we run this bench with and without fragmentation, we can see the effect that cache locality has on the deque. 

        <h2>Bench Results:</h2> 

        <ul>
            <li>Without Fragmentation, Forward Traversal: 0.175 seconds</li>
            <li>Without Fragmentation, Reverse Traversal: 0.146 seconds</li>
            <li>With Fragmentation Forward Traversal: 1.648 seconds</li>
            <li>With Fragmentation Reverse Traversal: 1.489 seconds</li>
        </ul>

        Fragmentation results in a slowdown of around 9-10x in this case. This resulted from just another deque being pushed to at the same time. <br><br>

        As these deque's are being malloc'd to the same heap, between two nodes in main_deque, there will be two trash nodes.<br><br>

        The deque with fragmentation will have a higher cache miss rate, and thus will be slower than the deque without fragmentation. <br><br>

        <h2>Profiling the Code</h2>

        When viewing the X86 Assembly for this bench, we get this result for the traversal (the benched part).

        <div class="code-block">
            <CodeBlock markdown={profile_used_payload} />
        </div>

        <h3>How Does This Factor Into Best Coding Practices?</h3>

        The deque's circular, double-sided nature makes it a suitable alternative to both linked-lists and stacks. Obviously many software systems rely on its efficient implementation. <br><br>

        While working with a deque, it is important to consider where memory is allocated. If a deque is being created or changed while other threads are malloc'ing to the same heap, it can be useful to rebuild it. <br><br>

        You could also consider forking a new process if the deque calculations are not reliant on the other deque's state. Process isolation means the child will have a different heap than the parent, but forking itself is a process on the order of 1ms, which might be much too long for certain applications.<br><br>

        More likely, for performance critical applications, using a hashmap that can prefetch cache more efficiently (as in an open-addressing system), since the hashmap will be contiguous in memory. <br><br>

        This involves reliance on a good hashing function for efficiency though, and having a high load factor can cause dramatic slowdown depending on the implementation of the hashtable. <br><br>
        <br><br><br>


        <button class="clickable-div" on:click={toggleExpand}>
            Testing Environment:
        </button> 

        {#if isExpanded}
            <ul class="testing-environment">
                <li>Windows 11</li>
                <li>16GB Ram</li>
                <li>AMD X86 Processor</li>
                <li>gcc 14.1.1</li>
            </ul>
        {:else}
            <br><br>
        {/if} 

        <h2>The Cache Locality Engineered Deque:</h2>

        The Cache Allocated Deque (affectionately called a "cleque") is a data structure I created to solve the nagging seeds of perfectionism in my head.

        As a result, I propose the cleque. You should not expect it to be: 
        <ul>
            <li>groundbreakingly fast</li>
            <li>memory efficient</li>
            <li>memory safe</li>
            <li>thread safe</li>
            <li>particularly practical for inclusion in most use cases</li>
        </ul>

        With all of these self-admitted shortcomings, and the cache locality engineered deque all but doomed to fail from its conception, is there still insight to be gained? I certainly believe so.


    </div>
</div>

<style>
    .all {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
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
