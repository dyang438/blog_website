<script>
    import CodeBlock from "../../CodeBlock.svelte";

    let isExpanded = false;

    function toggleExpand() {
        isExpanded = !isExpanded;
    }

    let profile_unused_payload = `\`\`\`c 
int x [MAIN_DEQUE_SIZE];

for (int i = 0; i < 100; i++)
{
	deque_node* front = main_deque->first;
    deque_node* iterfront = main_deque->first;
    while(iterfront->next != front) 
    {
        iterfront = iterfront->next;
        x[i] = iterfront->payload;
    }
}
\`\`\``;

    let assembly_payload = `\`\`\`X86
.L13:
        mov     rax, QWORD PTR [rax+8]
        cmp     rax, rdx
        jne     .L13
\`\`\``;

    let profile_fixed = `\`\`\`c
deque* trash = init_deque();
for (int i = 0; i < MAIN_DEQUE_SIZE; i++)
    deque_push_front(trash, i);
}
\`\`\``;
</script>

<div class="all">
    <br /><br />

    <h1 class="title">
        Making Sure to Profile Your Test Bench and Fighting Compilers
    </h1>

    <h2>Context:</h2>

    In my deque post, linked here: LINK the test bench depended on the size
    between pointers in the deque. This makes a difference when referencing
    different values within a struct, so it might have actually caused issues.
    <br /><br />

    This was the code:

    <div class="code-block">
        <CodeBlock markdown={profile_unused_payload} />
    </div>

    All this code does is iterate through the deque and store the payload in an
    array.<br /><br />

    This was the assembly from gcc 14.1.1 related to the loop that was given as
    a result when using the -O2 flag:

    <div class="code-block">
        <CodeBlock markdown={assembly_payload} />
    </div>

    Importantly, there is no point here where the compiler references the
    payload during iteration. The compiler realizes the stack-allocated x array
    is never used, and will go out of scope before being referenced.<br /><br />

    Thus, the values inside of the array that I store the payload values in were
    optimized out. I am trying to time it such that it is referencing this
    payload though, as the offset from the struct pointer might result in
    different cache behavior.<br /><br />

    The fix is quite simple (fortunately) -- just push the values into a deque
    that is never used.

    <div class="code-block">
        <CodeBlock markdown={profile_fixed} />
    </div>

    This likely works because the compiler cannot be *completely* sure that the
    deque is never used again, as it is a heap allocated object, so it needs to
    have a persistent lifetime even after the stack pointer returned from malloc
    goes out of scope.
    <br /><br />

    This also causes the optimize flag -O3 to reference the payload as well,
    meaning this was sufficient in my fight with the compiler. <br /><br />

    In future benches, I will make sure to continue profiling, but also figure
    out ways to bench without needing to fight gcc, if not, I'm sure after
    enough battles I'll lose one quite spectacularly!

    <br /><br /><br />
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
        <br /><br />
    {/if}
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
