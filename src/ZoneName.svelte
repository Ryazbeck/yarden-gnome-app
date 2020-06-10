<script>
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
  
  export let zoneDoc;
  export let zoneRef;
  let zoneNameUpdate;
  let editName = false;
</script>


<div class="flex flex-row items-center justify-between w-full h-auto px-4 py-2 text-xl font-bold text-white align-middle bg-green-500 border-t-2 border-green-300 rounded-t-sm">
  {#if editName}
    <input class="bg-green-500" bind:value={zoneDoc.name} />
    <button class="ml-2 text-base" on:click={() => zoneRef.update({
      name: zoneDoc.name,
      modified: Date.now(),
    }).then(() => editName=false)}>
      Save
    </button>
  {:else}
    <span>
      {zoneDoc.name}
    </span>
    <span on:click={() => editName=true} class="ml-2 cursor-pointer icon">
      <FaEdit />
    </span>
  {/if}
</div>


<style>
  .icon {
    color: #F0FFF4;
    width: 16px;
    height: 16px;
  }

  * {
    transition: .2s;
  }
</style>