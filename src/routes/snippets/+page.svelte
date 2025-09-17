<script>
    import ArticleItem from "../ArticleItem.svelte";

    let articles = [
        {
            title: "Testing O3 Compiler Level for GDB Debugging",
            description:
                "Exploring how aggressive compiler optimizations affect debugging experience with GDB.",
            time: "2025-09-16",
            href: "/snippets/o3_gdb_testing",
            underConstruction: true
        },
        {
            title: "Profiling rustc: Following the Dev Guide",
            description:
                "My experience following the rustc dev guide to profile Rust compiler performance.",
            time: "2025-09-11",
            href: "/snippets/rustc_profiling",
            underConstruction: true
        },
        {
            title: "Const in the Compiler",
            description:
                "How const declarations help compilers optimize code and improve performance.",
            time: "2025-09-11",
            href: "/snippets/const_compiler",
            underConstruction: true
        },
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
        underConstruction={article.underConstruction}
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
