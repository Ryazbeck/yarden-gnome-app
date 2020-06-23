<script>
  import moment from 'moment';
  import firebase from "firebase/app";
  import Chart from 'svelte-frappe-charts';
  import { onMount } from 'svelte';
  import { Doc, Collection } from "sveltefire";

  import Sensor from './Sensor.svelte'
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

  export let zoneRef;
  export let user;
</script>

<div class="mb-1 text-sm font-bold text-left text-white md:text-base">
  Sensors
</div>

<div class="md:flex md:flex-col md:justify-start">
  <Collection
    path={`sensors`}
    query={ref => ref.where('userId', '==', user.uid)
      .where('zoneId', '==', zoneRef.id).where('enabled', '==', true)}
    let:data={sensors}
    let:ref={sensorsRef}
    log>

    <Collection
      path={'moisture'}
      query={ref => ref.where('created', '>', getCreatedTime()).where('userId', '==', user.uid)
        .where('zoneId', '==', zoneRef.id).orderBy('created')}
      let:data={moistures}
      let:ref={moisturesRef}
      log>

      <div class="flex flex-col justify-start mb-4 md:flex-row md:justify-between">
        {#each sensors as sensor}
          <Sensor {user} {zoneRef} {sensor} />
        {/each}
      </div>
      
      <div class="mb-4">
        <div class="mb-1 text-sm font-bold text-left text-white md:text-base">
          History
        </div>

        <div class="bg-white rounded-lg shadow-lg">
          <Chart data={parseMoisture(moistures, sensors)} {...props} />
        </div>
      </div>

    </Collection>


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