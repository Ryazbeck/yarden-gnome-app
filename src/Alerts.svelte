<script>
  import moment from 'moment';
  import { Collection } from "sveltefire";
  import IoIosWater from 'svelte-icons/io/IoIosWater.svelte'

  export let user;
  export let zoneDoc;
</script>

<div class="mb-1 text-xs font-bold text-left text-white">
  Alerts
</div>

<Collection
  path={'alerts'}
  query={ref => ref.where('userId', '==', user.uid).where('acknowledged', '==', false).orderBy('created')}
  let:data={alerts}
  let:ref={alertsRef}
  log>

  {#each alerts as alert}
    <div class="flex flex-row mb-4 bg-white rounded-lg shadow-lg">
      <div class="w-1/5 p-4 text-red-400 fill-current">
        <IoIosWater />
      </div>
      <div class="flex flex-col w-4/5 py-3 my-auto text-xs leading-snug text-left">
        <span class="font-bold">
          Sensor {zoneDoc.sensors.indexOf(zoneDoc.sensors.find(snsr => snsr == alert.sensorId))+1}
        </span>
        <span>
          {alert.alertType}
        </span>
        <span>
          {moment(alert.created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
        </span>
      </div>
    </div>
  {/each}

</Collection>

<style>
  .sensor-icon {
    color: rgb(255, 103, 103);
  }
</style>