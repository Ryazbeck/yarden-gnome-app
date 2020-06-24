<script>
  import moment from 'moment';
  import firebase from "firebase/app";
  import Chart from 'svelte-frappe-charts';
  import { onMount } from 'svelte';
  import { Doc, Collection } from "sveltefire";
  import IoIosWater from 'svelte-icons/io/IoIosWater.svelte'
  import IoIosSunny from 'svelte-icons/io/IoIosSunny.svelte'

  let operators = {
    '<': function(a, b) { return a < b },
    '>': function(a, b) { return a > b }
  }

  let iconComponents = {
    "moisture": IoIosWater,
    "sunlight": IoIosSunny
  }

  const iconColor = {
    "moisture": (value, sensor) => operators[sensor.thresholdType](value, sensor.threshold) ? 'text-red-400' : 'text-blue-300',
    "sunlight": (value, sensor) => operators[sensor.thresholdType](value, sensor.threshold) ? 'text-gray-300' : 'text-yellow-500',
  }

  export let sensors;
  export let user;
</script>

<div class="mb-1 text-sm font-bold text-left text-white md:text-base">
  Sensors
</div>

<div class="flex flex-col justify-start mb-4 md:flex-row md:justify-between">
  {#if sensors.length}
    {#each sensors as sensor}
      <Collection
        path={'sensor-readings'}
        query={ref => ref.where('userId', '==', user.uid).where('sensorId', '==', sensor.ref.id).orderBy('created', 'desc').limit(1)}
        let:data={sensorReadings}
        let:ref={sensorReadingsRef}
        log>

          <div class="flex flex-row justify-center mb-0 bg-white rounded-lg shadow-lg md:w-5/12 sensor">
            <div class="w-1/5 p-4 fill-current {iconColor[sensor.sensorType](sensorReadings[0].value, sensor)}">
                <svelte:component this={iconComponents[sensor.sensorType]} />
            </div>
            <div class="w-3/5 h-auto py-3 my-auto text-xs leading-snug">
            <div class="font-bold text-left capitalize">
                {sensor.name}
            </div>
            <div class="text-left capitalize">
                {sensor.sensorType}
            </div>
            <div class="text-left">
                {moment(sensorReadings[0].created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
            </div>
            </div>
            <div class="flex w-1/5 text-2xl font-thin text-center align-middle">
            <div class="m-auto">
                {sensorReadings[0].value}%
            </div>
            </div>
          </div>

      </Collection>
    {/each}
  {:else}
    <div class="py-1 pl-2 my-auto text-xs font-bold text-left bg-white border-l-2 border-yellow-600 rounded-lg shadow-lg">
      There are no sensors associated with your account.
    </div>
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