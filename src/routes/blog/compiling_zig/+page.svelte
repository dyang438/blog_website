<script>
import CodeBlock from "../../CodeBlock.svelte";

let isExpanded = false;

function toggleExpand() {
    isExpanded = !isExpanded;
}

</script>


<div class="all">
<br><br>

    <h1 class="title">Learning and Profiling Zig, and an Aside on Why We Learn Programming Languages</h1>

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
