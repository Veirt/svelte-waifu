<script lang="ts">
    import { z } from "zod";
    import Button from "../components/Button.svelte";
    import Loading from "../components/Loading.svelte";
    import { PUBLIC_API_ENDPOINT } from "$env/static/public";

    export let activeCategory: string;

    const categorySchema = z.object({
        sfw: z.array(z.string()),
        nsfw: z.optional(z.array(z.string())),
    });

    async function getCategories() {
        const response = await fetch(`${PUBLIC_API_ENDPOINT}/endpoints`);

        if (!response.ok) {
            throw new Error("Something went wrong. Response is not OK.");
        }

        const data = await response.json();
        try {
            const categories = categorySchema.parse(data);
            return categories;
        } catch (err) {
            throw new Error(`Error when fetching categories: ${err}`);
        }
    }

    let categories = getCategories();

    function handleCategoryClick(category: string) {
        activeCategory = category;
    }
</script>

<div class="flex justify-center flex-wrap flex-column">
    {#await categories}
        <Loading />
    {:then categories}
        {#each categories.sfw as category}
            <Button
                active={activeCategory}
                {category}
                on:click={() => handleCategoryClick(category)}
                >{category}</Button
            >
        {/each}
    {:catch}
        <p>Failed</p>
    {/await}
</div>
