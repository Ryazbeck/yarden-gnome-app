<script>

  import Chart from 'svelte-frappe-charts';

  let data = {
    labels: ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'],
    datasets: [
      {
        name: 'Sensor 1',
        values: [75,70,68,63,60,55,50]
      },
      {
        name: 'Sensor 2',
        values: [74,71,66,62,62,58,50]
      },
    ],
    yRegions: [{
      label: "Moisture %",
      start: 0,
      end: 100
    }]
  };

  // export let zoneDoc;
</script>

<div class="mt-4 text-sm font-semibold text-left text-white text-small">
  Charts
</div>

<span class="w-1/2 mt-4 chart">
  <Chart data={data} type="line" />
</span>

<style>
  .chart > div {
    @apply bg-white;
  }
</style>