<script>
  import moment from 'moment';
  import MdAssignment from 'svelte-icons/md/MdAssignment.svelte'
  import firebase from "firebase/app";
  import { Collection } from "sveltefire";
  import Note from "./Note.svelte";
  import { Modal, Collapse, Button } from 'svelma'
  import FaPlus from 'svelte-icons/fa/FaPlus.svelte'
  import FaMinus from 'svelte-icons/fa/FaMinus.svelte'

  export let zoneRef;

  let addNote = false;
  let newNote;
  let notesOpen = false;
</script>

<div class="mt-4 text-sm font-semibold text-left text-white text-small">
  Notes
</div>

<Collection
  path={`${zoneRef.path}/notes`}
  query={ref => ref.orderBy('created', 'desc')}
  let:data={notes}
  let:ref={notesRef}
  log>

  <Collapse bind:open={notesOpen}>
    <div class="flex flex-row justify-start w-full p-2 font-bold text-left bg-white rounded-lg shadow-lg is-primary" slot="trigger">
      <span class="w-3 my-auto mr-1">
        {#if notesOpen}
          <FaMinus />
        {:else}
          <FaPlus />
        {/if}
      </span>
      <span class="h-full my-auto">
        {notesOpen ? 'collapse' : 'expand'}
      </span>
    </div>
    <div class="flex flex-col justify-start">
      <div class="flex flex-col {notes.length ? 'h-56' : 'h-12'} my-2 overflow-y-scroll md:pr-2 content">
        {#if notes.length}
          <ul class="mt-0 ml-0">
            {#each notes as note}
              <li>
                <span>
                {#if note.created}
                  <span class="flex flex-col justify-start text-left bg-white border-l-2 border-yellow-600 rounded-lg shadow-lg">
                    <div class="flex justify-between px-1 text-xs text-gray-600">
                      <span>
                        {moment(note.created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
                      </span>
                      <span on:click={() => note.ref.delete()}>
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
          <div class="my-auto text-base">
            No Notes...
          </div>
        {/if}
      </div>
      <div class="flex justify-end">
        <Button on:click={() => addNote = !addNote} class="font-bold text-white bg-green-300 is-small">
          Add Note
        </Button>
      </div>
    </div>
  </Collapse>

  <!-- create new note -->
  <Modal bind:active={addNote}>
    <div class="p-2 bg-green-100 border-4 border-green-400 shadow">
      Note:
      <textarea class="w-full h-20 p-1" bind:value={newNote} />
      <button class="p-1 mr-2 text-sm font-semibold text-white bg-blue-400 cursor-pointer hover:bg-blue-500" 
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

  * {
    transition: .2s;
  }
</style>