<script lang="ts">
    export let active: string;
    export let category: string;
</script>

<button
    on:click
    class:selected={active === category}
    class="border border-accent hover:bg-accent hover:transition-colors text-md font-medium text-white p-3"
>
    <slot />
</button>
