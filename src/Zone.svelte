<script>
  import firebase from "firebase/app";
  import { Doc } from "sveltefire";
  import ZoneName from "./ZoneName.svelte";
  import ZoneDescription from "./ZoneDescription.svelte";
  import Notes from "./Notes.svelte";
  import Charts from "./Charts.svelte";
  import Sensors from "./Sensors.svelte";
  import Crops from "./Crops.svelte";
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
  import { get } from 'svelte/store';
  import IoMdCloseCircle from 'svelte-icons/io/IoMdCloseCircle.svelte'
  import { Button, Modal, Collapse } from 'svelma'

  export let user;
  export let zone;
  export let selectedValue;

  let addCrops = false;
  let newCrops = null;
</script>

<Doc 
  path={`zones/${zone.id}`} 
  let:data={zoneDoc} 
  let:ref={zoneRef}
  >

  <ZoneName {zoneDoc} {zoneRef} bind:selectedValue={selectedValue} />

  <div class="w-full h-full px-4 pb-3 m-0">

    <ZoneDescription {zoneDoc} {zoneRef} />

    <Notes {zoneRef} />

    <!-- <Crops {zoneDoc} {zoneRef} /> -->
    
    <Charts {zoneDoc} />
  
    <!-- <Sensors {zoneDoc} {zoneRef} {user} /> -->

  </div>

  <!-- delete zone -->
  <div class="flex justify-end">
    <button class="p-1 mx-2 mb-2 text-white bg-red-300 hover:bg-red-400" 
      on:click|stopPropagation={() => zoneRef.delete()}>
      Delete zone
    </button>
  </div>

</Doc>
  
<style>
  * {
    transition: .2s;
  }
</style>