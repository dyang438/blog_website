<script>
    export let title;
    export let completionPercentage = 25;
</script>

<svelte:head>
    <title>{title}</title>
    <meta name="description" content="This post is not yet ready for viewing." />
</svelte:head>

<div class="all">
    <br /><br />

    <h1 class="title">
        {title}
    </h1>

    <div class="hidden-content">
        <h2>Not Enough is Showing</h2>
        <p>This post is currently only {completionPercentage}% complete and not ready for public viewing.</p>
        <p>Please check back later when more content is available!</p>

        <div class="progress-bar">
            <div class="progress-fill" style="width: {completionPercentage}%"></div>
        </div>
        <p class="progress-text">{completionPercentage}% Complete</p>
    </div>
</div>

<style>
    .all {
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
        max-width: 800px;
        margin: 0 auto;
        padding: 2rem;
    }

    .title {
        width: 100%;
        font-weight: bold;
        text-align: center;
        margin-bottom: 2rem;
    }

    .hidden-content {
        text-align: center;
        padding: 3rem 2rem;
        background-color: var(--color-bg-2);
        border-radius: 16px;
        border: 2px dashed #ccc;
    }


    h2 {
        color: #666;
        margin-bottom: 1rem;
    }

    p {
        color: #888;
        margin-bottom: 1rem;
        line-height: 1.6;
    }

    .progress-bar {
        width: 100%;
        height: 20px;
        background-color: #f0f0f0;
        border-radius: 10px;
        overflow: hidden;
        margin: 2rem 0 1rem 0;
        border: 1px solid #ddd;
    }

    .progress-fill {
        height: 100%;
        background: linear-gradient(90deg, #4caf50, #8bc34a);
        transition: width 0.3s ease;
        border-radius: 10px;
    }

    .progress-text {
        font-weight: 600;
        color: #666;
        margin-top: 0.5rem;
    }
</style>