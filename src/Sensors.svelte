<script>
  import { Collection } from "sveltefire";
  export let zone;
  export let user;

  console.log(zone)
</script>

{#each zone.sensors as sensor}
  <Collection
    path={`moisture`}
    query={ref => ref.where('userId', '==', user.uid)
      .where('zoneId', '==', zone.id).where('sensorId', '==', sensor)
      .orderBy('created')}
    let:data={moistures}
    let:ref={moisturesRef}
    log>

    {#each moistures as moisture}
      <span class="flex flex-col justify-center w-24 h-24 bg-white border-l-2 border-green-300 rounded shadow">
        <h2>Sensor {zone.sensors.indexOf(sensor)+1}</h2>
        {moisture.value}%
      </span>
    {/each}

  </Collection>
{/each}

