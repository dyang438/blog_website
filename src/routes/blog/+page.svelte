<script>
import ArticleItem from '../ArticleItem.svelte';

let articles = [
    {
        title: "Cache Locality, Branch Prediction, and the Deque",
        description: "Implementing a limited stack allocated deque, profiling the deque and its core operations.",
        time: "2024-07-11",
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
