<script>
  import moment from 'moment';
  import firebase from "firebase/app";
  import Chart from 'svelte-frappe-charts';
  import { onMount } from 'svelte';


  function parseMoisture(moistures, sensors) {
    return {
      labels: moistures.filter(v => v.sensorId == sensors[0].ref.id).map(v => moment(v.created.toDate()).format('LTS')),
      datasets: sensors.map(snsr => {
        return {
          name: snsr.name,
          chartType: 'line',
          values: moistures.filter(v => v.sensorId == snsr.ref.id).map(v => v.value)
        }
      }),
      yRegions: [{
        label: "Moisture range",
        start: 45,
        end: 80,
        options: {
          labelPos: 'left'
        }
      }],
    }
  }

  export let sensors;
  export let moistures;
  let props = {
    data: 0,
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

  onMount(() => props.data = parseMoisture(moistures, sensors))
</script>

<div class="mb-1 text-sm font-bold text-left text-white md:text-base">
  History
</div>

<div class="bg-white rounded-lg shadow-lg">

  {#if props.data}
    <Chart {...props} />
  {/if}
  
</div>

<style>
  .chart > div {
    @apply bg-white;
  }

  :global(.graph-svg-tip) {
    background: rgba(0,0,0,1) !important;
  }
</style>