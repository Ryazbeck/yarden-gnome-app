<script>
  import { Collection } from "sveltefire";
  import Zone from "./Zone.svelte";

  export let user;
  let zoneName;
  let zoneDescription;
</script>

<div class="max-w-screen-md mx-auto">
  <h1 class="w-1/4 py-2 m-auto my-4 text-2xl font-bold text-green-600 bg-gray-200 rounded shadow-sm">
    Zones
  </h1>

  <Collection
    path={'zones'}
    query={ref => ref.where('userId', '==', user.uid).orderBy('created')}
    let:data={zones}
    let:ref={zonesRef}
    log>

    {#if !zones.length}
        No zones yet...
    {/if}

    {#each zones as zone}
      <Zone {zone} />
    {/each}

    <div class="flex flex-col w-full bg-gray-200 border-2 border-green-500 rounded shadow-sm">
      <div class="flex flex-row">
        <div class="flex flex-col items-start w-1/2 p-2">
          <div class="flex flex-row w-full mb-2">
            <div class="w-1/3 pr-2 text-right">
              Name:
            </div>
            <div class="w-2/3 text-left">
              <input class="w-full" bind:value={zoneName} />
            </div>
          </div>
          <div class="flex flex-row w-full">
            <div class="w-1/3 pr-2 text-right">
              Description: 
            </div>
            <div class="w-2/3 text-left">
              <textarea class="w-full h-20" bind:value={zoneDescription} /><br />
            </div>
          </div>
        </div>

        <div class="flex flex-col items-start w-1/2 p-2">
        </div>
      </div>
      
      <div>
        <button class="p-1 mb-2 font-semibold text-white bg-green-400" on:click={() => zonesRef.add({
          name: zoneName,
          created: Date.now(),
          modified: Date.now(),
          description: zoneDescription,
          userId: user.uid
        })}>
          Create Zone
        </button>
      </div>
    </div>

  </Collection>
</div>

