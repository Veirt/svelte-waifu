<script lang="ts">
    import { onMount } from "svelte";

    export let src: string;

    let loaded = false;
    let failed = false;
    let loading = false;

    onMount(() => {
        const img = new Image();
        img.src = src;
        loading = true;

        img.onload = () => {
            loading = false;
            loaded = true;
        };

        img.onerror = () => {
            loading = false;
            failed = true;
        };
    });
</script>

{#if loaded}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
    <img
        on:click
        class="max-w-xs w-full bg-[#fff] object-cover object-top aspect-square m-3 cursor-pointer"
        {src}
        alt=""
    />
{:else if loading}
    <div
        class="max-w-xs w-full object-cover aspect-square m-3 bg-accent animate-pulse"
    />
{:else if failed}
    <img
        src="https://icon-library.com/images/not-found-icon/not-found-icon-20.jpg"
        alt="Not Found"
    />
{/if}
