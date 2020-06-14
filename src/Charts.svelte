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
      label: "Safe range",
      start: 45,
      end: 75,
      color: 'green',
    }]
  };

  export let zoneDoc;
  console.log(zoneDoc)
</script>

<div class="mb-1 text-xs font-bold text-left text-white">
  History
</div>

<div class="mb-5 bg-white rounded-lg shadow-lg">
  <Chart data={data} type="line" />
</div>

<style>
  .chart > div {
    @apply bg-white;
  }
</style>