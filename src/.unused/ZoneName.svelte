<script>
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
  
  export let zoneDoc;
  export let zoneRef;
  export let selectedValue;
  
  let zoneNameUpdate;
  let editName = false;
</script>

<!-- <img src="static/raised_planter.jpg" alt="zone" /> -->
<div class="flex flex-row w-full h-auto py-2 pl-10 text-2xl font-semibold text-left text-yellow-200">
  {#if editName}
    <input class="w-auto h-full p-1 text-center" bind:value={zoneDoc.name} />
    <button class="ml-2 text-base" on:click={() => zoneRef.update({
      name: zoneDoc.name,
      modified: Date.now(),
    }).then(() => editName=false)
      .then(() => selectedValue=zoneDoc.name)
    }>
      Save
    </button>
  {:else}
    <span>
      {zoneDoc.name}
    </span>
    <span on:click={() => editName=true} class="my-auto ml-2 cursor-pointer icon">
      <FaEdit />
    </span>
  {/if}
</div>

<style>
  .icon {
    color: #fffde8;
    width: 16px;
    height: 16px;
  }

  * {
    transition: .2s;
  }
</style>