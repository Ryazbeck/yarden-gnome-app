<script>
  import { Collection } from "sveltefire";
  import { Tabs, Tab } from 'svelma'
	import { tick } from 'svelte';
  // import Zones from "./Zones.svelte";
  import Zone from "./Zone.svelte";
  import ZoneCreate from "./ZoneCreate.svelte";

  export let user;
  let refreshing = false;
  let tabIndex = 0;

  async function refreshTabs(event) {
    refreshing = true;
    await tick();
    refreshing = false
    tabIndex = event.detail;
  }
</script>

<div class="max-w-screen-md mx-auto">
  <Collection
    path={'zones'}
    query={ref => ref.where('userId', '==', user.uid).orderBy('created')}
    let:data={zones}
    let:ref={zonesRef}
    log>

    {#if !zones.length}
        No zones yet...
    {/if}

    {#if !refreshing}
      <Tabs bind:value={tabIndex}>
        {#each zones as zone}
          <Tab label="{zone.name}">
            <Zone {zone} {user} on:refreshTabs="{refreshTabs}" />
          </Tab>
        {/each}
      </Tabs>
    {/if}

    <ZoneCreate {zonesRef} {user} {zones} on:refreshTabs={refreshTabs} />

  </Collection>
</div>

