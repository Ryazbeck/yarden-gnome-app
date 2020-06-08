<script>
  import { Doc } from "sveltefire";
  import Notes from "./Notes.svelte";
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'

  export let zone;
  let zoneNameUpdate;
  let editName = false;

  let zoneDescriptionUpdate;
  let editDescription = false;
</script>

<div class="w-full mx-auto mb-4 bg-gray-200 rounded shadow">
  <div class="flex flex-row items-center justify-start w-full h-auto py-2 pl-4 text-xl font-semibold text-white align-middle bg-green-500 rounded-t">
    {#if editName}
      <input class="bg-green-500" bind:value={zone.name} />
      <button class="ml-2 text-base" on:click={() => zone.ref.update({
        name: zone.name,
        modified: Date.now(),
      }).then(() => editName=false)}>
        save
      </button>
    {:else}
      <span>
        {zone.name}
      </span>
      <span on:click={() => editName=true} class="ml-2 cursor-pointer icon">
        <FaEdit />
      </span>
    {/if}
  </div>

  <!-- <div>
    Created: <em>{new Date(zone.created).toLocaleString()}</em><br />
    Modified: <em>{new Date(zone.modified).toLocaleString()}</em>
  </div> -->

  <!-- zone description -->
  <div class="flex flex-row items-center justify-start w-auto h-auto p-2 m-2 bg-green-100 rounded">
    {#if editDescription}
      <input class="bg-green-100" bind:value={zone.description} />
      <button class="ml-2 text-base" on:click={() => zone.ref.update({
        description: zone.description,
        modified: Date.now(),
      }).then(() => editDescription=false)}>
        save
      </button>
    {:else}
      <span>
        {zone.description}
      </span>
      <span on:click={() => editDescription=true} class="ml-2 cursor-pointer icon">
        <FaEdit />
      </span>
    {/if}
  </div>

  <Notes {zone} />

  <!-- delete zone -->
  <div class="flex justify-end">
    <button class="p-1 mx-2 mb-2 text-white bg-red-300 hover:bg-red-400" on:click={() => zone.ref.delete()}>Delete zone</button>
  </div>
</div>



<style>
  .icon {
    color: gray;
    width: 16px;
    height: 16px;
  }

  * {
    transition: .2s;
  }
</style>