<script>
  import { Doc } from "sveltefire";
  import ZoneName from "./ZoneName.svelte";
  import ZoneDescription from "./ZoneDescription.svelte";
  import Notes from "./Notes.svelte";
  import Charts from "./Charts.svelte";
  import Sensors from "./Sensors.svelte";
  import Crops from "./Crops.svelte";
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
	import { createEventDispatcher } from 'svelte';
	import { tick } from 'svelte';

  export let zone;
  export let zones;
  export let user;

  const dispatch = createEventDispatcher();
  
  let refreshing = false;

  function refreshTabs(tabIndex) {
    dispatch('refreshTabs', tabIndex);
  }

  async function refreshZone(event) {
    refreshing = true;
    await tick();
    refreshing = false
  }
  
  async function deleteZone(zoneRef) {
    await zoneRef.delete();
    refreshTabs(zones.indexOf(zones.find(z => z = zone)));
  }
</script>

<div class="w-full mx-auto mb-4 bg-gray-200 border-b-2 border-green-300 rounded shadow">

  <Doc 
    path={`zones/${zone.id}`} 
    let:data={zoneDoc} 
    let:ref={zoneRef}
    >

      <ZoneName {zoneDoc} {zoneRef} />

      <div class="w-full h-full p-2 pb-3 m-0">

        <ZoneDescription {zoneDoc} {zoneRef} />

    {#if !refreshing}
        <div class="flex flex-col justify-start w-full pt-3 mt-2">
          <Crops {zoneDoc} {zoneRef} on:refreshZone="{refreshZone}" />
        </div>
    {/if}

        <div class="flex w-full pt-3 mt-2 justify-evenly">
          <span class="w-1/2 pt-1 mr-2 bg-white border-2">
            <Notes {zoneRef} />
          </span>

          <span class="w-1/2">
            <Charts {zoneDoc} />
          </span>
        </div>
      
        {#if zoneDoc.sensors}
          <div class="flex w-full pt-3 mt-2 sensors justify-evenly">
            <Sensors {zoneDoc} {zoneRef} {user} />
          </div>
        {/if}

      </div>

    <!-- delete zone -->
    <div class="flex justify-end">
      <button class="p-1 mx-2 mb-2 text-white bg-red-300 hover:bg-red-400" 
        on:click|stopPropagation={() => deleteZone(zoneRef)}>
        Delete zone
      </button>
    </div>

  </Doc>
  
</div>

<style>
  .sensors {
    @apply px-4;
  }
</style>