<script>
  import moment from 'moment';
  import MdAssignment from 'svelte-icons/md/MdAssignment.svelte'
  import firebase from "firebase/app";
  import { Collection } from "sveltefire";
  import Note from "./Note.svelte";
  import { Modal, Collapse, Button } from 'svelma'
  import FaPlus from 'svelte-icons/fa/FaPlus.svelte'
  import FaMinus from 'svelte-icons/fa/FaMinus.svelte'

  export let user;
  let addNote = false;
  let newNote;
  let notesOpen = false;
</script>

<div class="mb-1 font-bold text-left text-white">
  <span class="text-sm md:text-base">
    Notes
  </span>
  <span class="float-right text-xs cursor-pointer md:text-sm hover:text-blue-300" on:click={() => addNote = !addNote}>
    Add note
  </span>
</div>

<Collection
  path={`notes/`}
  query={ref => ref.where('userId', '==', user.uid).orderBy('created', 'desc')}
  let:data={notes}
  let:ref={notesRef}
  log>

    <div class="flex flex-col justify-start">
      <div class="flex flex-col h-auto overflow-y-scroll md:pr-2">
        {#if notes.length}
          <ul class="mt-0 ml-0">
            {#each notes as note}
              <li>
                <span>
                {#if note.created}
                  <span class="flex flex-col justify-start p-1 text-left bg-white border-l-2 border-yellow-600 rounded-lg shadow-lg">
                    <div class="flex justify-between px-1 text-xs text-gray-600">
                      <span>
                        {moment(note.created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
                      </span>
                      <span on:click={() => note.ref.delete()} class="cursor-pointer hover:text-blue-300">
                        delete
                      </span>
                    </div>
                    <div class="p-3 text-sm font-semibold text-gray-700">
                      {note.note}
                    </div>
                  </span>
                {:else}
                  loading...
                {/if}
              </li>
            {/each}
          </ul>
        {:else}
          <div class="py-1 pl-2 my-auto mb-4 text-xs font-bold text-left bg-white border-l-2 border-yellow-600 rounded-lg shadow-lg">
            Click 'add note' to create a note
          </div>
        {/if}
      </div>
    </div>

  <!-- create new note -->
  <Modal bind:active={addNote}>
    <div class="p-2 mx-2 bg-green-100 border-4 border-green-400 shadow md:mx-0">
      Note:
      <textarea class="w-full h-20 p-1" bind:value={newNote} />
      <button class="p-1 mr-2 text-sm font-semibold text-white bg-blue-400 cursor-pointer hover:bg-blue-300" 
        on:click={() => notesRef.add({
          created: firebase.firestore.FieldValue.serverTimestamp(),
          note: newNote
      }).then(() => addNote = !addNote)}>
        Save Note
      </button>
    </div>
  </Modal>

</Collection>

<style>
  .icon {
    color: #525252;
    width: 10px;
    height: 10px;
  }

  li:not(:last-child) {
    @apply mb-3;
  }

  * {
    transition: .2s;
  }
</style>