<script>
    import CodeBlock from "../../CodeBlock.svelte";

    let isExpanded = false;

    function toggleExpand() {
        isExpanded = !isExpanded;
    }

    let array_vs_linked_list = `\`\`\`c++ 
using namespace std //let me be this is a demonstration

int main() {
    

}

\`\`\``;
</script>

<div class="all">
    <br /><br />

    <h1 class="title">Understanding Cache Modules</h1>

    <h2>The Data Hierarchy</h2>

    <img
        src={"../"}
        alt="diagram not here yet, because I'm making my own!"
    /><br />

    The cache (or SRAM) in your system has normally 3 levels, L1, L2, and L3.
    The cache is directly built on the CPU module, leading to extremely fast
    accesses and is a large reason for the speed of modern programs. <br /><br
    />

    The cache stores data that has been fetched from your DRAM (system memory)
    and holds it until the cache is filled. This applies to both data and
    instructions that are pulled from memory, leading to the separate data and
    instruction caches both having separate, but similarly powerful effects on
    performance.
    <br /><br />

    When the same instruction or data reference is accessed multiple times in a
    row, we take advantage of temporal locality, which means the data is
    unlikely to have left cache by the time it is referenced again. <br /><br />

    Spatial locality is based off the principle that a reference in memory is
    likely to be accessed along with its neighbors (think iterating through an
    array). Thus, the cache prefetches 64 bytes (1 cache line), along with any
    reference that was pulled from DRAM. This causes data that is small and
    spaced close to take advantage of cache easily as more data points can be
    prefetched at once.<br /><br />

    Having to go out to DRAM is called a cache-miss, and it has huge
    ramifications for performance versus programs that can effectively take
    advantage of cache. To demonstrate, let's look at the classic example for
    how big a difference cache makes.

    <h3>Array-Lists to Linked-Lists.</h3>

    Isn't insertion from the front supposed to be much much faster on a linked
    list? Isn't deletion from the front? Only if you're looking at algorithmic
    complexity.<br /><br />

    Let's run the following C++ bench comparing std::vector to std::queue in
    operations that queue should be doing better in.

    <div class="code-block">
        <CodeBlock markdown={array_vs_linked_list} />
    </div>

    How much do you expect one to outpace the other?

    <!-- 
    HERE WE PUT A GRAPH
    -->

    I was utterly flabbergasted seeing this demonstration for the first time --
    if you're coming from my deque post, it's quite obvious why I would want to
    make the deque take advantage of cache from here, but it seems like a pipe
    dream compared to the array list. <br /><br />

    Hopefully this demonstration gives you a reason to believe in benchmarking
    your code, rather than just believing that CPU's and algorithms run at
    asymptotic speeds when sample sizes are at all large.

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
