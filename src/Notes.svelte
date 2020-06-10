<script>
  import firebase from "firebase/app";
  import { Collection } from "sveltefire";
  import Note from "./Note.svelte";
  import { Modal } from 'svelma'

  export let zone;

  let addNote = false;
  let newNote;
</script>

<span class="w-1/2 text-sm border-green-400 rounded-sm">
  <div class="flex flex-row justify-between w-auto mb-1 align-middle rounded-sm">
    <div class="w-1/6 p-1 font-semibold">
      Notes
    </div>
    <div on:click={() => addNote = !addNote} class="w-20 p-1 mr-2 font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500">
      Add Note
    </div>
  </div>

  <div class="h-56 px-1 overflow-scroll overflow-x-hidden">
    <Collection
      path={`${zone.ref.path}/notes`}
      query={ref => ref.orderBy('created', 'desc')}
      let:data={notes}
      let:ref={notesRef}
      log>

      {#if notes.length}
        {#each notes as note}
          <Note {note} />
        {/each}
      {:else}
        This zone has no notes
      {/if}

      <Modal bind:active={addNote}>
        <div class="p-2 bg-white bg-green-100 border-4 border-green-400 rounded shadow">
          Note:
          <textarea class="w-full h-20 p-1" bind:value={newNote} />
          <button class="p-1 mr-2 text-sm font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500" 
            on:click={() => notesRef.add({
              created: firebase.firestore.FieldValue.serverTimestamp(),
              note: newNote
          }).then(() => addNote = !addNote)}>
            Save Note
          </button>
        </div>
      </Modal>

    </Collection>
  </div>
</span>

<style>
  * {
    transition: .2s;
  }
</style>