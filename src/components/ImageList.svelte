<script lang="ts">
    import { PUBLIC_API_ENDPOINT } from "$env/static/public";
    import { z } from "zod";
    import Modal from "./Modal.svelte";
    import Image from "./Image.svelte";
    import { browser } from "$app/environment";

    export let activeCategory: string;
    let exclude: string[] = [];

    const imagesSchema = z.object({ files: z.array(z.string()) });

    async function fetchImages(category: string) {
        const response = await fetch(
            `${PUBLIC_API_ENDPOINT}/many/sfw/${category}`,
            {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({ exclude }),
            }
        );

        if (!response.ok) {
            throw new Error("Something went wrong. Response is not OK.");
        }

        try {
            const data = await response.json();
            const parsedData = imagesSchema.parse(data);
            return parsedData.files;
        } catch (err) {
            throw new Error(`Error when fetching images: ${err}`);
        }
    }

    let showModal = false;
    let modalImage: string = "";

    let promiseImages: Promise<string[]> = Promise.resolve([]);

    $: if (browser) promiseImages = fetchImages(activeCategory);
</script>

<svelte:head>
    {#if showModal}
        <style>
            body {
                overflow: hidden;
            }
        </style>
    {/if}
</svelte:head>

<div class="my-5 flex justify-center flex-wrap">
    {#await promiseImages then images}
        {#each images as image}
            <!-- svelte-ignore a11y-no-noninteractive-element-interactions a11y-click-events-have-key-events -->
            <Image
                src={image}
                on:click={() => {
                    showModal = true;
                    modalImage = image;
                }}
            />
        {/each}
    {/await}
</div>

<Modal bind:showModal>
    <img
        class="aspect-auto md:max-w-xl max-h-96 md:max-h-screen"
        src={modalImage}
        alt=""
    />
</Modal>
