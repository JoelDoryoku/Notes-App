<script type="module">
    import { writable } from 'svelte/store';

    export let searchValue = writable('');
</script>

<input type="text" name="search" on:input={(e) => {searchValue = writable(e.target.value)}}>