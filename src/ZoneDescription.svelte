<script>
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
  
  export let zone;
  let zoneDescriptionUpdate;
  let editDescription = false;
</script>


<div class="flex flex-row items-center justify-between w-auto h-auto p-2 bg-green-100 rounded-sm">
  {#if editDescription}
    <input class="font-semibold bg-green-100" bind:value={zone.description} />
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


<style>
  .icon {
    color: #48BB78;
    width: 16px;
    height: 16px;
  }

  * {
    transition: .2s;
  }
</style>