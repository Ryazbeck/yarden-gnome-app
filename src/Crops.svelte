<script>
  import firebase from "firebase/app";
  import IoMdCloseCircle from 'svelte-icons/io/IoMdCloseCircle.svelte'
  import { Button, Modal } from 'svelma'
	import { tick } from 'svelte';
	import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let zoneDoc;
  export let zoneRef;
  let crops = zoneDoc.crops;
  let addCrops = false;
  let newCrops = null;
  let refreshing = false;

  function refreshZone(tabIndex) {
    dispatch('refreshZone', tabIndex);
  }
  
  async function refreshCrops() {
    refreshing = true;
    await tick();
    refreshing = false;
    crops = zoneDoc.crops;
  }
</script>

<div class="flex flex-col justify-start w-full h-auto p-2 text-sm bg-white border-2 border-gray-300 rounded-sm">
  <div class="flex flex-row justify-between w-auto mb-1 align-middle rounded-sm">
    <div class="p-1 font-semibold">
      Crops
    </div>
    <div on:click={() => addCrops = !addCrops} class="w-20 p-1 font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500">
      Add Crops
    </div>
  </div>
  <div class="flex flex-row justify-start h-8">
    {#if crops}
      {#if crops.length}
        {#each crops as crop}
          <span class="p-1 px-2 mr-2 font-semibold text-white bg-green-500 border-2 border-green-600 rounded-full">
            {crop} 
            <span class="align-middle cursor-pointer icon" on:click={() => {
                zoneRef.update({
                  modified: firebase.firestore.FieldValue.serverTimestamp(),
                  crops: crops.filter(cropEl => cropEl != crop)
              }).then(() => refreshZone())}}>
              <IoMdCloseCircle />
            </span>
          </span>
        {/each}
      {:else}
        <span class="p-1 px-2 align-middle">
          No crops created for this zone.
        </span>
      {/if}
    {/if}
  </div>
</div>

<Modal bind:active={addCrops}>
  <div class="p-2 bg-white bg-green-100 border-4 border-green-400 rounded shadow">
    Crops (comma separated):
    <input class="w-full p-1 mb-1" bind:value={newCrops} />
    <button class="p-1 mr-2 text-sm font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500" 
      on:click={() => zoneRef.update({
        modified: firebase.firestore.FieldValue.serverTimestamp(),
        crops: crops ? crops.concat(newCrops.split(",")) : newCrops.split(/\s*,\s*/)
    }).then(() => addCrops = !addCrops).then(() => refreshCrops())}>
      Save Note
    </button>
  </div>
</Modal>


<style>
  .icon {
    transition: .2s;
    @apply h-4 w-4;
  }

  .icon:hover {
    color: lightcoral;
  }
</style>