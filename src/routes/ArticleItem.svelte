<script>
    export let title;
    export let description;
    export let time;
    export let href;
    export let underConstruction = false;
    export let completionPercentage = 100;
    export let hidden = false;
    export let snippet = false;
    export let noLink = false;
</script>

<div
	class="article"
	class:snippet-article={snippet}
	class:hidden-article={hidden}
	class:article-static={noLink}
>
	<svelte:element
		this={noLink ? 'div' : 'a'}
		{...noLink ? {} : { href }}
		class="article-link"
	>
		<div class="article-top">
			<h3>{title}</h3>
			<div class="status-indicators">
				{#if completionPercentage < 100}
					<div class="completion-percentage">{completionPercentage}%</div>
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
			<p class="article-desc">{description}</p>
		{/if}
		<p class="article-date">{time}</p>
	</svelte:element>
</div>

<style>
    .article {
        border-radius: 6px;
        border: 1px solid rgba(0, 0, 0, 0.07);
        background-color: var(--color-bg-2);
        margin: 0.5rem 1rem;
        transition: box-shadow 0.2s ease, border-color 0.2s ease;
    }

    .article:hover:not(.article-static) {
        border-color: rgba(0, 0, 0, 0.13);
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.06);
    }

    .article-static {
        cursor: default;
    }

    .article-link {
        display: flex;
        flex-direction: column;
        padding: 1.1rem 1.5rem;
        text-decoration: none;
        color: inherit;
    }

    .article-top {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        gap: 1rem;
        width: 100%;
    }

    h3 {
        margin: 0 0 0.35rem;
        font-size: 1rem;
        font-weight: 600;
        color: rgba(0, 0, 0, 0.88);
    }

    .article-desc {
        font-size: 0.9rem;
        color: rgba(0, 0, 0, 0.6);
        margin: 0 0 0.4rem;
        line-height: 1.55;
    }

    .article-date {
        font-size: 0.8rem;
        color: rgba(0, 0, 0, 0.35);
        margin: 0;
        font-weight: 500;
    }

    .hidden-article {
        opacity: 0.6;
    }

    .snippet-article {
        margin: 0.3rem 1rem;
    }

    .snippet-article h3 {
        font-size: 0.92rem;
    }

    .status-indicators {
        display: flex;
        gap: 0.4rem;
        align-items: center;
        flex-shrink: 0;
        margin-top: 0.1rem;
    }

    .completion-percentage {
        background: rgba(0, 0, 0, 0.05);
        color: rgba(0, 0, 0, 0.45);
        border-radius: 4px;
        padding: 0.15rem 0.45rem;
        font-size: 0.72rem;
        font-weight: 600;
        white-space: nowrap;
    }

    .under-construction {
        background: rgba(0, 0, 0, 0.05);
        color: rgba(0, 0, 0, 0.45);
        border-radius: 4px;
        padding: 0.15rem 0.45rem;
        font-size: 0.72rem;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.4px;
        white-space: nowrap;
    }

    .snippet-badge {
        background: rgba(192, 57, 43, 0.07);
        color: #c0392b;
        border-radius: 4px;
        padding: 0.15rem 0.45rem;
        font-size: 0.72rem;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.4px;
        white-space: nowrap;
    }
</style>
