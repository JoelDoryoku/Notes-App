<script>
    let notesArray = [];
    let newNoteElement;
    let newNoteText;

    if (typeof window !== 'undefined') {
        if (localStorage.getItem('index') === null) {
            localStorage.setItem('index', 0);
        }

        getNotes();
    }

    function newNote() {
        newNoteElement.classList.remove('hidden');
    }

    function saveNote() {
        localStorage.setItem(localStorage.getItem('index'), newNoteText.value);
        localStorage.setItem('index', parseInt(localStorage.getItem('index')) + 1);

        newNoteText.value = '';
        newNoteElement.classList.add('hidden');

        getNotes();
    }

    function cancelNote() {
        newNoteElement.classList.add('hidden');
        newNoteText.value = '';
    }

    function getNotes() {
        notesArray = [];

        for (let i = 0; i < localStorage.getItem('index'); i++) {
            notesArray.push(localStorage.getItem(i));
        }

        notesArray = notesArray;
    }
</script>

{#each notesArray as note}
<div>{note}</div>
{/each}

<button on:click={newNote} class="bg-green-700 text-center w-full h-10 rounded-xl text-lg hover:bg-green-600 transition-all">
    <i class="fas fa-plus text-white"></i> <i class="font-bold text-white not-italic">New Note</i>
</button>

<div id="newNote" class="absolute top-0 left-0 w-screen h-screen bg-neutral-500/50 flex justify-center items-center hidden" bind:this={newNoteElement}>
    <div class="h-1/2 w-1/4">
        <!-- Note Form -->
        <div class="w-full">
            <textarea class="w-full h-full rounded-xl p-4" cols="30" rows="10" bind:this={newNoteText}></textarea>
        </div>

        <!-- Buttons -->
        <div class="flex justify-between pt-4">
            <button on:click={saveNote} class="newNoteButton bg-green-500">Create</button>
            <button on:click={cancelNote} class="newNoteButton bg-red-500">Cancel</button>
        </div>
    </div>
</div>