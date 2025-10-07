<script>
import ArticleItem from '../ArticleItem.svelte';

let articles = [
    {
        title: "Pushing a Paper in a Month: My Winter at Reticular AI (YC F'24)",
        description: "My experience contributing to AI research and publishing at a Y Combinator startup.",
        time: "2025-09-16",
        href: "/blog/reticular_ai_winter",
        underConstruction: true,
        completionPercentage: 70,
        hidden: false
    },
    {
        title: "My Summer at Amazon",
        description: "Reflections on my summer internship experience at Amazon.",
        time: "2025-09-16",
        href: "/blog/amazon_summer",
        underConstruction: true,
        completionPercentage: 25,
        hidden: true
    },
    {
        title: "Opinionated Software and Rust's Borrow Checker",
        description: "Examples of C++ that don't work in Rust, mostly for good reason.",
        time: "2025-09-11",
        href: "/blog/rust_borrow_checker",
        underConstruction: true,
        completionPercentage: 25,
        hidden: true
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
    }
]
</script>

<div>
<br><br>

{#each articles as article}
    <ArticleItem title={article.title} description={article.description} time={article.time} href={article.href} underConstruction={article.underConstruction} completionPercentage={article.completionPercentage} hidden={article.hidden} />
{/each}

</div>
