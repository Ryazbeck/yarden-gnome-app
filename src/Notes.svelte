<script>
  import { Collection } from "sveltefire";
  import Note from "./Note.svelte";

  export let zone;
</script>

<span class="w-1/2 text-sm border-green-400 rounded-sm">
  <div class="flex flex-row justify-start justify-between w-auto mb-1 align-middle rounded-sm">
    <div class="w-1/6 p-1 font-semibold">
      Notes
    </div>
    <div class="w-20 p-1 font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500">
      Add Note
    </div>
  </div>

  <div>
    <Collection
      path={`${zone.ref.path}/notes`}
      query={ref => ref.orderBy('created', 'desc')}
      let:data={notes}
      let:ref={notesRef}
      log>

      {#each notes as note}
        <Note {note} />
      {/each}

    </Collection>
  </div>
  <!-- <span class="ml-1 cursor-pointer icon">
    <span on:click={() => editName=true} >
    <IoIosAdd />
  </span> -->
</span>


<style>
  * {
    transition: .2s;
  }
</style>