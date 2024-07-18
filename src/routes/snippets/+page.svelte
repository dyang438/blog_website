<script>
import ArticleItem from '../ArticleItem.svelte';

let articles = [
    {
        title: "On Makefiles",
        description: null,
        time: "2024-07-11",
        href: "/snippets/makefiles"
    }

]
</script>

<div><br><br></div>

<div id="disclaimer">
    The point of a snippet is to just write what I notice about a topic, either because my knowledge is so rudimentary no key insights are to be gained, or testing a far out idea rigorously just isn't a priority. If a writing here is unfinished I'll color it differently.
</div>

{#each articles as article}
    <ArticleItem title={article.title} description={article.description} time={article.time} href={article.href} />
{/each}

<style>
    #disclaimer {
        font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        color: black;
        text-decoration: none;
        width: 100%;
        margin-left: 1em;
        margin-right: 1em;
    }

</style>
