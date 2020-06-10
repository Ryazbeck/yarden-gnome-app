<script>
  import ZoneName from "./ZoneName.svelte";
  import ZoneDescription from "./ZoneDescription.svelte";
  import Notes from "./Notes.svelte";
  import Charts from "./Charts.svelte";
  import Sensors from "./Sensors.svelte";
  import Crops from "./Crops.svelte";
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
	import { createEventDispatcher } from 'svelte';

  export let zone;
  export let user;

  const dispatch = createEventDispatcher();

  function refreshTabs(tabIndex) {
    dispatch('refreshTabs', tabIndex);
  }
  
  async function deleteZone(zone) {
    await zone.ref.delete();
    refreshTabs(0);
  }
</script>

<div class="w-full mx-auto mb-4 bg-gray-200 border-b-2 border-green-300 rounded shadow">

  <ZoneName {zone} />

  <div class="w-full h-full p-2 pb-3 m-0">

    <ZoneDescription {zone} />

    <div class="flex flex-col justify-start w-full pt-3 mt-2">
      <Crops {zone} />
    </div>

    <div class="flex w-full pt-3 mt-2 justify-evenly">
      <span class="w-1/2 pt-1 mr-2 bg-white border-2">
        <Notes {zone} />
      </span>

      <span class="w-1/2">
        <Charts {zone} />
      </span>
    </div>
  
    {#if zone.sensors}
      <div class="flex w-full pt-3 mt-2 sensors justify-evenly">
        <Sensors {zone} {user} />
      </div>
    {/if}

  </div>

  <!-- delete zone -->
  <div class="flex justify-end">
    <button class="p-1 mx-2 mb-2 text-white bg-red-300 hover:bg-red-400" 
      on:click|stopPropagation={() => deleteZone(zone)}>
      Delete zone
    </button>
  </div>
  
</div>

<style>
  .sensors {
    @apply px-4;
  }
</style>