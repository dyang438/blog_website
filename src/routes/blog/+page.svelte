<script>
import ArticleItem from '../ArticleItem.svelte';

let articles = [
    // {
    //    title: "Why we Learn Programming Languages + the Zig llvm Compiler",
    //    description: "Finding Unique ways to use Zig and comparing the assembly to its closest cousin in C.",
    //    time: "2024-07-22",
    //    href: "/blog/compiling_zig"
    // },
    {
        title: "Cache Locality, Malloc, and the Deque",
        description: "Implementing a limited stack allocated deque, profiling the deque and its core operations.",
        time: "2024-07-22",
        href: "/blog/deque"
    }
]
</script>

<div>
<br><br>

{#each articles as article}
    <ArticleItem title={article.title} description={article.description} time={article.time} href={article.href} />
{/each}

</div>
