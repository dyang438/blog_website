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
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
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
    font-family: "Courier New", Courier, monospace;
  }
</style>
