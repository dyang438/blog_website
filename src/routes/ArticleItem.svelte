<script>
    export let title;
    export let description;
    export let time;
    export let href;
</script>

<div class="article">
    <nav class="inside_box">
        <a href="{href}" class="inside_text">
            <div id="top">
                <h3>{title}</h3>
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
        border-radius: 30px; /* Adjusted border-radius to be consistent */
        border: black;
        display: flex;
        color: black;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding: 2.5em;
        padding-bottom: 1em;
        padding-top: 1em;
        margin: 1em;
        background-color: var(--color-bg-2);
    }

    .article:hover {
        background-color: var(--color-bg-1);
    }
</style>
