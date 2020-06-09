<script>
  import { Doc } from "sveltefire";
  import ZoneName from "./ZoneName.svelte";
  import ZoneDescription from "./ZoneDescription.svelte";
  import Notes from "./Notes.svelte";
  import Charts from "./Charts.svelte";
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'

  export let zone;
</script>

<div class="w-full mx-auto mb-4 bg-gray-200 border-b-2 border-green-300 rounded shadow">

  <!-- <div>
    Created: <em>{new Date(zone.created).toLocaleString()}</em><br />
    Modified: <em>{new Date(zone.modified).toLocaleString()}</em>
  </div> -->

  <ZoneName {zone} />

  <div class="w-full h-full p-2 pb-3 m-0">

    <ZoneDescription {zone} />

    <div class="flex w-full pt-3 mt-2 justify-evenly">
      <span class="w-1/2 mr-2">
        <Notes {zone} />
      </span>

      <span class="w-1/2">
        <Charts {zone} />
      </span>
    </div>
  
  </div>

  <!-- delete zone -->
  <div class="flex justify-end">
    <button class="p-1 mx-2 mb-2 text-white bg-red-300 hover:bg-red-400" on:click={() => zone.ref.delete()}>Delete zone</button>
  </div>
</div>