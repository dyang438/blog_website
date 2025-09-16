<script>
  import { onMount, afterUpdate } from "svelte";
  import { marked } from "marked";
  import hljs from "highlight.js";
  import "highlight.js/styles/github-dark.css";

  export let markdown = "";

  let htmlContent = "";

  marked.setOptions({
    highlight: function (code, lang) {
      const language = hljs.getLanguage(lang) ? lang : "plaintext";
      return hljs.highlight(code, { language }).value;
    },
  });

  //reactive statement or else the code unmounts on refresh.
  $: htmlContent = marked(markdown);

  onMount(() => {
    htmlContent = marked(markdown);
  });

  afterUpdate(() => {
    // Re-highlight code blocks after the component updates
    document.querySelectorAll("pre code").forEach((block) => {
      hljs.highlightElement(block);
    });
  });
</script>

<div class="markdown-content">
  {@html htmlContent}
</div>

<style>
  .markdown-content {
    margin: 15px 0;
  }

  :global(pre) {
    background-color: #1e1e1e !important;
    color: #d4d4d4 !important;
    padding: 20px !important;
    border-radius: 8px !important;
    overflow-x: auto !important;
    font-size: 14px !important;
    line-height: 1.5 !important;
    border: 1px solid #333 !important;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1) !important;
  }

  :global(code) {
    font-family: 'Fira Mono', 'Courier New', Courier, monospace !important;
    font-size: 14px !important;
  }

  :global(pre code) {
    background: none !important;
    padding: 0 !important;
    border-radius: 0 !important;
    color: inherit !important;
  }
</style>
