<script>
  import FaEdit from 'svelte-icons/fa/FaEdit.svelte'
  import { tick } from 'svelte'
  export let zoneDoc;
  export let zoneRef;
  zoneDoc.description = zoneDoc.description ? zoneDoc.description : '';
  let editDescription = false;

  function init(el){
    el.focus()
  }
</script>

<div class="mt-4 text-sm font-semibold text-left text-white text-small">
  Description
</div>

<div class="flex flex-row items-center justify-between w-auto h-auto bg-white rounded-lg shadow-lg">
  {#if editDescription}
    <input class="w-full h-full p-1 ml-1 font-thin border-0 outline-none" bind:value={zoneDoc.description} placeholder="Zone description" use:init />
    <button class="h-8 px-1 m-1 text-base text-white bg-green-400 hover:bg-green-500" on:click={() => zoneRef.update({
      description: zoneDoc.description,
      modified: Date.now(),
    }).then(() => editDescription=false)}>
      save
    </button>
  {:else}
    <span class="p-2 font-thin">
      {zoneDoc.description}
    </span>
    <span id="description" on:click={() => editDescription=true} class="m-2 cursor-pointer icon">
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