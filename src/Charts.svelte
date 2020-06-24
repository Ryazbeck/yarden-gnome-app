<script>
  import moment from 'moment';
  import firebase from "firebase/app";
  import Chart from 'svelte-frappe-charts';
  import { onMount } from 'svelte';
  import { Doc, Collection } from "sveltefire";

  function parseData(datas, sensors) {
    return {
      labels: datas.filter(v => v.sensorId == sensors[0].ref.id).map(v => moment(v.created.toDate()).format('LTS')),
      datasets: sensors.map(snsr => {
        return {
          name: snsr.name,
          chartType: 'line',
          values: datas.filter(v => v.sensorId == snsr.ref.id).map(v => v.value)
        }
      }),
    }
  }

  let props = {
    colors: ['light-blue', 'yellow', 'purple'],
    axisOptions: {
      xAxisMode: "tick",
      xIsSeries: true
    },
    tooltipOptions: {
      formatTooltipX: d => (d + '').toUpperCase(),
      formatTooltipY: d => d + '%',
    }
  }

  function getCreatedTime() {
    return moment().subtract(4,'h').toDate()
  }

  export let sensors;
  export let user;
</script>

<div class="mb-1 text-sm font-bold text-left text-white md:text-base">
  History
</div>

<div class="mb-4 md:flex md:flex-col md:justify-start">

  <Collection
    path={'sensor-readings'}
    query={ref => ref.where('created', '>', getCreatedTime()).where('userId', '==', user.uid).orderBy('created')}
    let:data={sensorReadings}
    let:ref={sensorReadingsRef}
    log>

    <div class="bg-white rounded-lg shadow-lg">
      {#if sensorReadings.length}
        <Chart data={parseData(sensorReadings, sensors)} {...props} />
      {:else}
        <div class="py-1 pl-2 my-auto text-xs font-bold text-left bg-white border-l-2 border-yellow-600 rounded-lg shadow-lg">
          Your sensors haven't recorded any data yet.
        </div>
      {/if}      
    </div>

  </Collection>

</div>

<style>

  .chart > div {
    @apply bg-white;
  }

  :global(.graph-svg-tip) {
    background: rgba(0,0,0,1) !important;
  }
</style>