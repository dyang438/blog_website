<script>
import ArticleItem from '../ArticleItem.svelte';

let articles = [
    {
        title: "Agent Construction",
        description: "",
        time: "2026-03-20",
        href: "/blog/agent_construction",
        underConstruction: true,
        completionPercentage: 0,
        hidden: true,
        noLink: true
    },
    {
        title: "Part of Amazon's Agentic Movement",
        description: "Reflections on my summer internship experience at Amazon.",
        time: "2025-09-16",
        href: "/blog/amazon_summer",
        underConstruction: false,
        completionPercentage: 100,
        hidden: false
    },
    // {
    //     title: "Testing O3 Compiler Level for GDB Debugging",
    //     description: "Exploring how aggressive compiler optimizations affect debugging experience with GDB.",
    //     time: "2025-09-16",
    //     href: "/snippets/o3_gdb_testing",
    //     underConstruction: true,
    //     snippet: true,
    //     hidden: false
    // },
    // {
    //     title: "Opinionated Software and Rust's Borrow Checker",
    //     description: "Examples of C++ that don't work in Rust, mostly for good reason.",
    //     time: "2025-09-11",
    //     href: "/blog/rust_borrow_checker",
    //     underConstruction: true,
    //     completionPercentage: 25,
    //     hidden: false
    // },
    {
        title: "Const in the Compiler",
        description: "How const declarations help compilers optimize code and improve performance.",
        time: "2025-09-11",
        href: "/snippets/const_compiler",
        snippet: true,
        hidden: false
    },
    {
        title: "Scaling a Model in a Month: ML Engineering at Reticular AI (YC F'24)",
        description: "My experience contributing to Interpretable ML research and publishing at a Y Combinator startup.",
        time: "2025-03-05",
        href: "/blog/reticular_ai_winter",
        underConstruction: false,
        completionPercentage: 100,
        hidden: false
    },
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
        href: "/blog/deque",
        underConstruction: false,
        completionPercentage: 100,
        hidden: false
    },
    {
        title: "Cache and Why We Need It",
        description: "Very introductory spiel on my favorite part of the CPU (next to the branch predictor).",
        time: "2024-07-22",
        href: "/snippets/cache_locality",
        snippet: true,
        hidden: false
    },
    {
        title: "A Reason to Profile Code",
        description: "A consequence of the deque post that taught me yet another way I can make mistakes.",
        time: "2024-07-22",
        href: "/snippets/compiler_opt",
        snippet: true,
        hidden: false
    },
]
</script>

<div class="blog-page">
<br><br>

<div class="source-link">
    <a href="https://github.com/dyang438/blog_investigations" target="_blank" rel="noopener noreferrer">Source Code — all investigation code and examples</a>
</div>

{#each articles.filter(a => !a.snippet) as article}
    <ArticleItem title={article.title} description={article.description} time={article.time} href={article.href} underConstruction={article.underConstruction} completionPercentage={article.completionPercentage} hidden={article.hidden} snippet={article.snippet} noLink={article.noLink ?? false} />
{/each}
<div class="snippet-divider">
    <p>The point of a snippet is to compile thoughts for certain things I've learned that aren't groundbreaking but I enjoyed learning about them so I like to pass them around.</p>
</div>
{#each articles.filter(a => a.snippet) as article}
    <ArticleItem title={article.title} description={article.description} time={article.time} href={article.href} underConstruction={article.underConstruction} completionPercentage={article.completionPercentage} hidden={article.hidden} snippet={article.snippet} noLink={article.noLink ?? false} />
{/each}

</div>

<style>
    .snippet-divider {
        border-top: 1px solid rgba(0, 0, 0, 0.1);
        margin: 1.5rem 1rem 0.5rem 1rem;
        padding-top: 1.5rem;
        text-align: center;
        color: rgba(0, 0, 0, 0.5);
        font-size: 0.9rem;
        font-style: italic;
    }

    .snippet-divider p {
        margin: 0;
    }

    .blog-page {
        max-width: 820px;
        margin: 0 auto;
    }

    .source-link {
        text-align: center;
        margin: 0 1rem 1.5rem;
        font-size: 0.9rem;
    }
</style>
