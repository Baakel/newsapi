<script>
    export let active;
</script>

<button on:click|preventDefault class="py-2 px-3 bg-gradient-to-br from-green-400 to-blue-400 rounded-lg text-neutral-800 font-bold pointer relative" class:active>
    <slot></slot>
</button>

<style>
    button.active {
        @apply bg-gradient-to-br from-red-400 to-purple-400;
    }
</style>