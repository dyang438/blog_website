<script>
  import { onMount } from 'svelte';
  import { marked } from 'marked';

  export let markdown = '';

  let htmlContent = '';

  onMount(() => {
    htmlContent = marked(markdown);
  });
</script>

<div class="markdown-content">
  {@html htmlContent}
</div>

<style>
  .markdown-content {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    padding: 0px;
    border-radius: 5px;
    overflow-x: auto;
  }

  :global(pre) {
    background-color: #272822;
    color: #f8f8f2;
    padding: 10px;
    border-radius: 5px;
    overflow-x: auto;
  }

  :global(code) {
    font-family: 'Courier New', Courier, monospace;
  }
</style>
