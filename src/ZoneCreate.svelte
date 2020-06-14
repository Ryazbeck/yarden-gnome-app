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
      crops: cropsList ? cropsList.split(",") : []
    }).then(() => dispatch('refreshTabs', zones.length-1));

    let zoneNameInputEl = document.getElementById('zoneNameInput');
    let zoneDescriptionInputEl = document.getElementById('zoneDescriptionInput');
    let cropsListInputEl = document.getElementById('cropsListInput');
  }
</script>

<div class="flex flex-col w-full mb-2 bg-gray-200 border-2 border-green-500 rounded shadow-sm">
  <form>
    <div class="flex flex-col items-start w-full p-2 text-sm">
      <div class="flex flex-row w-full mb-2">
        <div class="w-full pr-2 text-left">
          Name:
          <input id="zoneNameInput" class="w-full" bind:value={zoneName} />
        </div>
      </div>
      <div class="flex flex-row w-full">
        <div class="w-full pr-2 text-left">
          Description: 
          <textarea id="zoneDescriptionInput" class="w-full h-20" bind:value={zoneDescription} />
        </div>
      </div>
      <div class="flex flex-row w-full">
        <div class="w-full pr-2 text-left">
          Crops (comma separated):
          <input id="cropsListInput" class="w-full" bind:value={cropsList} />
        </div>
      </div>
    </div>
    
    <div>
      <button type="reset" class="p-1 mb-2 text-sm font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500" 
        on:click|stopPropagation={() => addZone(zoneName, zoneDescription, cropsList)}>
        Create Zone
      </button>
    </div>
  </form>
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