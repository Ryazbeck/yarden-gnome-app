<script>
  import firebase from "firebase/app";
  import IoMdCloseCircle from 'svelte-icons/io/IoMdCloseCircle.svelte'
  import { Button, Modal, Collapse } from 'svelma'
	import { tick } from 'svelte';
	import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let zoneDoc;
  export let zoneRef;
  let crops = zoneDoc.crops;
  let addCrops = false;
  let newCrops = null;
  let refreshing = false;

  async function refreshMe() {
    refreshing = true;
    setTimeout(() => {
      refreshing = false;
    }, 200)
  }
</script>


<span class="w-full p-2 mt-2 font-bold text-left text-white bg-green-400 outline-none hover:bg-green-500">
  Crops
</span>
<div class="px-4 mt-2 bg-white rounded">
  <Button on:click={() => addCrops = !addCrops} class="my-2 font-bold text-white bg-green-300 is-small">
    Add Crop
  </Button>
  <div class="p-2 content">
    {#if crops}
    {#if crops.length}
      <ul>
      {#each crops as crop}
        <li class="flex justify-between p-1 px-2 mr-2 font-semibold text-white bg-green-500 border-2 border-green-600">
          <span>{crop}</span>
          <span class="my-auto align-middle cursor-pointer icon" on:click={() => {
              zoneRef.update({
                modified: firebase.firestore.FieldValue.serverTimestamp(),
                crops: crops.filter(cropEl => cropEl != crop)
              }).then(refreshMe())}}>
            <IoMdCloseCircle />
          </span>
        </li>
      {/each}
      </ul>
    {:else}
      This zone has no crops
    {/if}
    {/if}
  </div>
</div>

<!-- <div class="flex flex-col justify-start w-full h-auto p-2 text-sm bg-white border-2 border-gray-300 rounded-sm">
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
        {/each}
      {:else}
        <span class="p-1 px-2 align-middle">
          This zone has no crops assigned.
        </span>
      {/if}
    {/if}
  </div>
</div> -->

<Modal bind:active={addCrops}>
  <div class="p-2 bg-white bg-green-100 border-4 border-green-400 rounded shadow">
    Crops (comma separated):
    <input class="w-full p-1 mb-1" bind:value={newCrops} />
    <button class="p-1 mr-2 text-sm font-semibold text-white bg-blue-400 rounded-sm cursor-pointer hover:bg-blue-500" 
      on:click={() => zoneRef.update({
        modified: firebase.firestore.FieldValue.serverTimestamp(),
        crops: crops ? crops.concat(newCrops.split(",")) : newCrops.split(/\s*,\s*/)
    }).then(() => addCrops = !addCrops).then(refreshMe())}>
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