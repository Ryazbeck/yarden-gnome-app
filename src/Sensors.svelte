<script>
  import { Collection } from "sveltefire";
  
  export let zoneDoc;
  export let zoneRef;
  export let user;

  console.log(zoneDoc, zoneDoc.id, zoneRef.id)
</script>

{#each zoneDoc.sensors as sensor}
  <Collection
    path={`moisture`}
    query={ref => ref.where('userId', '==', user.uid)
      .where('zoneId', '==', zoneRef.id).where('sensorId', '==', sensor)
      .orderBy('created')}
    let:data={moistures}
    let:ref={moisturesRef}
    log>

    {#each moistures as moisture}
      <span class="flex flex-col justify-center w-full bg-white border-l-2 border-green-300 rounded shadow sensor">
        <div class="top-0 flex w-full h-10 mt-0 font-bold align-middle bg-green-200 text-middle">
          <div class="w-full h-auto my-auto text-center align-middle">
            Sensor {zoneDoc.sensors.indexOf(sensor)+1}
          </div>
        </div>
        <div class="flex flex-col justify-center h-32">
          <div class="text-sm">
            Soil Moisture
          </div>
          <div class="text-3xl">
            {moisture.value}%
          </div>
        </div>
      </span>
    {/each}

  </Collection>
{/each}

<style>
  .sensor:not(:last-child) {
    @apply mr-6;
  }
</style>