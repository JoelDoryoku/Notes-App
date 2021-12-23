<script context="module">
    import { writable } from 'svelte/store';

    export let searchValue = writable('');
</script>

<input class="rounded-xl w-full p-2" type="text" name="search" placeholder="Search by date..." on:input={(e) => {$searchValue = e.target.value}}>