<script>
    //import CodeBlock from "../../CodeBlock.svelte";

    let isExpanded = false;

    function toggleExpand() {
        isExpanded = !isExpanded;
    }
</script>

<div class="all">
    <br /><br />

    <h1 class="title">
        Learning and Profiling Zig, and an Aside on Why We Learn Programming
        Languages
    </h1>

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
    /*
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
*/
</style>
