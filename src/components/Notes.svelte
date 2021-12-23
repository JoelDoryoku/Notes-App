<script>
    import { searchValue } from './Search.svelte';

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
        newNoteText.focus();
    }

    function saveNote() {
        if (newNoteText.value != "") {
            localStorage.setItem(localStorage.getItem('index'), `${newNoteText.value}&${new Date().toLocaleString()}`);
            localStorage.setItem('index', parseInt(localStorage.getItem('index')) + 1);

            newNoteText.value = '';
            newNoteElement.classList.add('hidden');

            getNotes();
        } else {
            newNoteText.value = 'Please enter a note';
            newNoteText.focus();
        }
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

    function deleteNote(index) {
        localStorage.setItem(index, "");

        cleanIndex();
        getNotes();
    }

    function splitNote(note, charToSplit, index) {
        return note.split(charToSplit)[index];
    }

    function cleanIndex() {
        let emptyIndex;

        for (let i = 0; i < localStorage.getItem('index'); i++) {
            if (localStorage.getItem(i) == "") {
                emptyIndex = i;
                break;
            }
        }

        if (emptyIndex != undefined) {
            for (let i = emptyIndex; i < localStorage.getItem('index'); i++) {
                localStorage.setItem(i, localStorage.getItem(i + 1));
            }

            localStorage.setItem('index', parseInt(localStorage.getItem('index')) - 1);
        }
    }
</script>

{#each notesArray as note, index}
<div class="border-2 border-neutral-800 rounded-xl w-full h-64 overflow-hidden bg-neutral-200/80">
    <div class="border-b-2 border-neutral-800 flex justify-between">
        <header class="p-4 font-bold">
        {#if splitNote(note, "&", 1) != undefined}
            {splitNote(note, "&", 1)}
        {:else}
            Date doesn't exist :(
        {/if}
        </header>

        <button on:click={() => {deleteNote(index)}} class="bg-red-700 p-4 hover:bg-red-600"><i class="fas fa-trash text-white"></i></button>
    </div>
    <p class="m-4 h-40 overflow-hidden break-words">{splitNote(note, "&", 0)}</p>
</div>
{/each}

<button on:click={newNote} class="bg-green-700 absolute bottom-4 right-4 text-center w-52 h-14 rounded-xl text-lg hover:bg-green-600 transition-all">
    <i class="fas fa-plus text-white"></i> <i class="font-bold text-white not-italic">New Note</i>
</button>

<div id="newNote" class="absolute top-0 left-0 w-screen h-screen bg-neutral-500/50 flex justify-center items-center hidden" bind:this={newNoteElement}>
    <div class="h-1/2 w-1/4">
        <!-- Note Form -->
        <div class="w-full">
            <textarea class="w-full h-full rounded-xl p-4 auto" cols="30" rows="10" bind:this={newNoteText}></textarea>
        </div>

        <!-- Buttons -->
        <div class="flex justify-between pt-4">
            <button on:click={saveNote} class="newNoteButton bg-green-500">Create</button>
            <button on:click={cancelNote} class="newNoteButton bg-red-500">Cancel</button>
        </div>
    </div>
</div>