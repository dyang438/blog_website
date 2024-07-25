<script>
    import CodeBlock from "../../CodeBlock.svelte";

    let isExpanded = false;

    function toggleExpand() {
        isExpanded = !isExpanded;
    }

    let array_vs_linked_list = `\`\`\`c++ 
//instantiate empty vector and list
std::vector sorted_vec; 
std::list sorted_list;

for (int i = 0; i < TEST_SIZE; i++) {
    //generate a random number in a large range we set with udist.
    int rand_num = (int) udist(rng);

    //we insert into the proper spot to keep the vector sorted
    sorted_vec.insert_sorted(rand_num); 
}

for (int i = 0; i < TEST_SIZE; i++) {
    //generate a random number in a large range we set with udist.
    int rand_num = (int) udist(rng);

    //we insert into the proper spot to keep the list sorted
    sorted_list.insert_sorted(rand_num); 
}
\`\`\``;
</script>

<div class="all">
    <br /><br />

    <h1 class="title">Understanding Cache Modules</h1>

    <h2>The Data Hierarchy</h2>

    Here is a fancy diagram ripped from my class lecture notes explaining the
    role of cpu cache and its relation to other storage devices.

    <br />(credits to Travis Mcgaha and Bryant and O'Hallaron). <br />

    <img
        src={"/images/data-hierarchy.png"}
        alt="this was taken from my cis3800 lecture notes"
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

    Isn't insertion faster on a linked list than on an array list? Isn't
    deletion? Only if you're looking at algorithmic complexity.<br /><br />

    Let's run the following C++ bench comparing std::vector to std::queue in
    operations that queue should be doing better in.

    <div class="code-block">
        <CodeBlock markdown={array_vs_linked_list} />
    </div>

    How much do we expect one to outpace the other? At what point will the
    linked list run faster than the array list?

    <img src={"/images/vector_random.png"} alt="insertion test" />
    <br />

    <img src={"/images/stdlist_random.png"} alt="deletion test" />
    <br />

    I was genuinely flabbergasted seeing this demonstration for the first time.
    <br /><br />

    As a quick analysis, the vector operations run hundreds of times faster, and
    scale better with volume than the list operation as well. If you're coming
    from my deque post, it's quite obvious why it's important to make sure your
    calls to malloc help the deque take advantage of cache, but unfortunately no
    pointer-based list can compare to the array list. This is why C++'s deque
    implementation actually relies on using an internal array like a vector, and
    allows resizing on both ends as an alternative to allocating each node
    individually. <br /><br />

    This demonstration gives another reason to believe in benchmarking code,
    CPU's and algorithms don't run at asymptotic speeds, nor is the data
    structure choice the end all be all, it's just about how fast it runs.

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
    img {
        width: 40%;
    }
</style>
