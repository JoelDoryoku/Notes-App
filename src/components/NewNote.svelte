<script>
    let newNoteElement;

    function newNote() {
        newNoteElement.classList.remove('hidden');
    }
</script>

<button on:click={newNote} class="bg-green-700 text-center w-full h-10 rounded-full text-lg hover:bg-green-600 transition-all">
    <i class="fas fa-plus text-white"></i> <i class="font-bold text-white not-italic">New Note</i>
</button>

<div id="newNote" class="hidden" bind:this={newNoteElement}>

</div>