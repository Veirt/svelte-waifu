<script lang="ts">
    import Category from "../components/Category.svelte";
    import ImageList from "../components/ImageList.svelte";

    let activeCategory = "waifu";
</script>

<div class="flex justify-center">
    <div class="max-w-6xl flex justify-center flex-nowrap flex-col">
        <Category bind:activeCategory />
        <ImageList bind:activeCategory />
    </div>
</div>
