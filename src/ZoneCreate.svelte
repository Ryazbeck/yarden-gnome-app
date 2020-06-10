<script>
  import firebase from "firebase/app";
  import { createEventDispatcher } from 'svelte';
  
  export let user;
  export let zonesRef;
  export let zones;
  let zoneName;
  let zoneDescription;
  let cropsList;

  const dispatch = createEventDispatcher();
  
  async function addZone(zoneName, zoneDescription, cropsList) {
    await zonesRef.add({
      name: zoneName,
      created: firebase.firestore.FieldValue.serverTimestamp(),
      modified: firebase.firestore.FieldValue.serverTimestamp(),
      description: zoneDescription,
      userId: user.uid,
      crops: cropsList.split(",")
    })
    .then(ref => ref.get()
    // focus on the tab that was just created
    .then(zone => dispatch('refreshTabs', zones.indexOf(zones.find(z => z.id == zone.id)))))
    .then(() => {
      zoneName = null;
      zoneDescription = null;
      cropsList = null;
    })
  }
</script>

<div class="flex flex-col w-full mb-2 bg-gray-200 border-2 border-green-500 rounded shadow-sm">
  <div class="flex flex-col items-start w-full p-2 text-sm">
    <div class="flex flex-row w-full mb-2">
      <div class="w-full pr-2 text-left">
        Name:
        <input class="w-full" bind:value={zoneName} />
      </div>
    </div>
    <div class="flex flex-row w-full">
      <div class="w-full pr-2 text-left">
        Description: 
        <textarea class="w-full h-20" bind:value={zoneDescription} />
      </div>
    </div>
    <div class="flex flex-row w-full">
      <div class="w-full pr-2 text-left">
        Crops (comma separated):
        <input class="w-full" bind:value={cropsList} />
      </div>
    </div>
  </div>
  
  <div>
    <button class="p-1 mb-2 text-sm font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500" 
      on:click|stopPropagation={() => addZone(zoneName, zoneDescription, cropsList)}>
      Create Zone
    </button>
  </div>
</div>

<style>
  input,
  textarea {
    @apply p-1 rounded border-green-300;
  }

  input:focus,
  textarea:focus {
    @apply bg-green-100 border-2;
    outline: none;
  }
</style>