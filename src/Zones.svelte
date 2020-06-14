<script>
  import firebase from "firebase/app";
  import { Collection } from "sveltefire";
  import { Collapse } from 'svelma'
	import { onMount } from 'svelte';
  import ZoneDescription from "./ZoneDescription.svelte";
  // import Zones from "./Zones.svelte";
  import Zone from "./Zone.svelte";
  import ZoneCreate from "./ZoneCreate.svelte";
  import Select from 'svelte-select';

  export let user;
  let selectedValue = undefined;
  let items;

  function selectZones(zones) {
    items = zones.map((zone) => {
      return {
        value: zone.name,
        label: zone.name
      }
    })
    selectedValue = items[0].value
    return items;
  }

  let selectProps = {
    placeholder: "Click to Select or Create a Zone",
    isCreatable: true,
    noOptionsMessage: 'No Zones, click to create',
    showChevron: true
  }

  async function handleSelect(selectedValue, zonesRef) {
    if (!items.find(val => val.value == selectedValue.value)) {
      await zonesRef.add({ 
        name: selectedValue.value,
        userId: user.uid,
        created: firebase.firestore.FieldValue.serverTimestamp(),
        modified: firebase.firestore.FieldValue.serverTimestamp(),
      })
      zonesRef = zonesRef
    }
  }
</script>

<div class="max-w-screen-md mx-auto">
  <Collection
    path={'zones'}
    query={ref => ref.where('userId', '==', user.uid).orderBy('created')}
    let:data={zones}
    let:ref={zonesRef}
    log>
    

    <div class="container px-4 pb-2 mb-2">
      <Select 
        items={selectZones(zones)} 
        on:select={() => handleSelect(selectedValue, zonesRef)} 
        {...selectProps} 
        bind:selectedValue
      >
      </Select>
    </div>

    {#if !zones.length}
        No zones yet...
    {/if}

    {#if selectedValue}
      {#each zones as zone}
        {#if selectedValue.value == zone.name}
          <Zone bind:selectedValue={selectedValue} {zone} {user} />
        {/if}
      {/each}
    {/if}

    <span slot="loading">Loading Zones...</span>

    <!-- <ZoneCreate {zonesRef} {user} {zones} /> -->

  </Collection>
</div>

