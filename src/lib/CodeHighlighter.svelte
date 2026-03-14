<script>
    import {codeToHtml} from 'shiki'

    let copyStatus = $state('Copy');

    async function highlightCode(code, lang) {
        // noinspection ES6RedundantAwait
        return await codeToHtml(code, {
            lang: lang,
            theme: theme,
            transformers: []
        })
    }

    async function copyToClipboard() {
        await navigator.clipboard.writeText(code);
        copyStatus = 'Copied!';
        setTimeout(() => {
            copyStatus = 'Copy';
        }, 1500);
    }

    let {code, lang = 'html', theme = 'vitesse-dark', showCopyButton = true} = $props();

    let html = $state('');

    $effect(async () => {
        html = await highlightCode(code, lang);
    })

</script>
<div class="shiki-svelte" style="position: relative">
    {#if showCopyButton}
        <button onclick={copyToClipboard}>{copyStatus}</button>
        <div>
            {@html html}
        </div>
    {:else}
        {@html html}
    {/if}
</div>
<style>


    button {
        position: absolute;
        top: 10px;
        right: 10px;
        background-color: #000;
        color: #fff;
        padding: 5px 10px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: small;
        opacity: 0.5;
    }

</style>
