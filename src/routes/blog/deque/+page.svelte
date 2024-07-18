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
	if (!input || !input->first) {
		exit(-1); 
	}

	int ret = input->first->payload;
	input->first->next->prev = input->last;
	input->last->next = input->first->next;
	input->first = input->first->next;
	input->length--;
	return ret;
}
\`\`\``;

</script>

<div class="all">
<br><br>

    <h1 class="title">Profiling the Deque, how effective is Cache Locality in the Heap?</h1>


    <div> 
        Cache Locality often dominates algorithmic complexity when considering runtime factors, with the classic example being linked-lists versus array-lists/vectors.

        There are many writings already on the subject of cache misses and reference locality, which would be good intuition to have existing before reading this. (One interesting post I recently read is linked <a href="https://fylux.github.io/2017/06/29/list_vs_vector/">here</a>)<br><br>

        Instead, this post will focus on how <code>malloc()</code> practically allocates memory and how/if you can make a deque take advantage of cache. 

        <h3>Current Deque Implementations:</h3>

        Here I'm going to be implementing a classic deque in C, and then profiling the code in x86 assembly.

        <h2>C Deque with Heap Allocation</h2>

        <div class="code-block">
            <CodeBlock markdown={deque_struct} />
        </div>

        
        
        The deque's circular, double-sided nature makes it a suitable alternative to both linked-lists and stacks.

        Let us take a look at a pushing operation:

        <div class="code-block">
            <CodeBlock markdown={deque_push} />
        </div>

        Aside from all of the pointer manipulation, the large feature of this operation is the malloc'ing of a new node. This means that every node is created on the heap using a relatively costly call to malloc.
        Here intuition suggests pushing consecutively to a deque multiple times would create a locality-friendly arrangement of nodes we can assume malloc will allocate nodes one after another.<br><br>

        Let's also take a look at a popping operation:
        <div class="code-block">
            <CodeBlock markdown={deque_pop} />
        </div>

        The key insight here is that after a call to pop_front and push_front, malloc wouldn't have any reason to allocate the pushed node to the same place we popped from, meaning less branch predictability. <br> <br>

        <h2>Lessons on Malloc</h2>

        The testing algorithm consists of a lot of 

        <button class="clickable-div" on:click={toggleExpand}>
            Testing Environment:
        </button> 

        {#if isExpanded}
            <ul class="testing-environment">
                <li>Arch Linux VM</li>
                <li>8GB DDR5 Ram allocated to the VM</li>
                <li>AMD X86 Processor</li>
                <li>gcc 14.1.1</li>
            </ul>
        {:else}
            <br><br>
        {/if} 

        
          
        



<!--
        - possibly look at the source code for the C++ STL deque<br>
        - possibly look at the source code for the Rust deque<br>
        - possibly look at the source code for the Python deque<br>
-->
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
