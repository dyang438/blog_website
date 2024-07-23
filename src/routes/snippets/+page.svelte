<script>
    import ArticleItem from "../ArticleItem.svelte";

    let articles = [
        {
            title: "Cache and Why We Need It",
            description:
                "Very introductory spiel on my favorite part of the CPU (next to the branch predictor).",
            time: "2024-07-22",
            href: "/snippets/cache_locality",
        },
        {
            title: "A Reason to Profile Code",
            description:
                "A consequence of the deque post that taught me yet another way I can make mistakes.",
            time: "2024-07-22",
            href: "/snippets/compiler_opt",
        },
    ];
</script>

<div><br /><br /></div>

<div id="disclaimer">
    The point of a snippet is to just write answers for questions when I'm
    unconfident about a topic, or little things to supplement a big post. <br
    />Also, sometimes rigorously testing a far out idea just isn't a priority.
</div>

{#each articles as article}
    <ArticleItem
        title={article.title}
        description={article.description}
        time={article.time}
        href={article.href}
    />
{/each}

<style>
    #disclaimer {
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
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
