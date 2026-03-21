<script>
    export let title;
    export let description;
    export let time;
    export let href;
    export let underConstruction = false;
    export let completionPercentage = 100;
    export let hidden = false;
    export let snippet = false;
</script>

<div class="article" class:snippet-article={snippet}>
    <nav class="inside_box">
        <a {href} class="inside_text">
            <div id="top">
                <h3>{title}</h3>
                <div class="status-indicators">
                    {#if completionPercentage < 100}
                        <div class="completion-percentage">{completionPercentage}% Complete</div>
                    {/if}
                    {#if underConstruction}
                        <div class="under-construction">Under Construction</div>
                    {/if}
                    {#if snippet}
                        <div class="snippet-badge">Snippet</div>
                    {/if}
                </div>
            </div>
            {#if description}
                <p style="text-align: justify">{description}</p>
            {/if}
            <p style="font-weight: normal;">{time}</p>
        </a>
    </nav>
</div>

<style>
    h3 {
        margin-top: 0.5em;
        margin-bottom: 0.5em;
    }

    p {
        margin-top: 0.5em;
        margin-bottom: 0.5em;
    }

    #top {
        display: flex;
        justify-content: space-between;
        width: 100%;
        margin-bottom: 0;
    }

    .inside_text {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-start; /* Changed to flex-start */
        text-align: left;
        color: black;
        text-decoration: none;
        width: 100%;
        margin-left: 1em;
        margin-right: 1em;
    }

    .inside_box {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-align: left;
        color: black;
        text-decoration: none;
        width: 100%;
    }

    .article {
        border-radius: 12px;
        border: 1px solid rgba(0, 0, 0, 0.07);
        display: flex;
        color: black;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding: 1.5em 2.5em;
        margin: 0.6em 1em;
        background-color: var(--color-bg-2);
        transition: box-shadow 0.2s ease, border-color 0.2s ease;
    }

    .article:hover {
        background-color: var(--color-bg-1);
        border-color: rgba(0, 0, 0, 0.15);
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
    }

    .snippet-article {
        padding: 0.75em 2.5em;
        margin: 0.3em 1em;
        opacity: 0.8;
    }

    .snippet-article h3 {
        font-size: 0.95rem;
    }

    .status-indicators {
        display: flex;
        gap: 0.5rem;
        margin-left: auto;
        align-items: center;
    }

    .completion-percentage {
        background-color: #e1f5fe;
        color: #0277bd;
        border: 1px solid #81d4fa;
        border-radius: 8px;
        padding: 0.25rem 0.5rem;
        font-size: 0.75rem;
        font-weight: 600;
        white-space: nowrap;
    }

    .under-construction {
        background-color: #fff3cd;
        color: #856404;
        border: 1px solid #ffeaa7;
        border-radius: 8px;
        padding: 0.25rem 0.5rem;
        font-size: 0.75rem;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.5px;
        white-space: nowrap;
    }

    .snippet-badge {
        background-color: #ffebee;
        color: #b71c1c;
        border: 1px solid #ef9a9a;
        border-radius: 8px;
        padding: 0.25rem 0.5rem;
        font-size: 0.75rem;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.5px;
        white-space: nowrap;
    }
</style>
