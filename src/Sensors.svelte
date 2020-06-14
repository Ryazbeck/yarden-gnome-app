<script>
  import moment from 'moment';
  import { Doc, Collection } from "sveltefire";
  import IoIosWater from 'svelte-icons/io/IoIosWater.svelte'

  export let zoneDoc;
  export let zoneRef;
  export let user;

  var operators = {
    '<': function(a, b) { return a < b },
    '>': function(a, b) { return a > b }
  }

  function iconColor(value, threshold, type) {
    return operators[type](value, threshold)
  }
</script>

<div class="mb-1 text-xs font-bold text-left text-white">
  Sensors
</div>

<div class="mb-5 md:flex md:flex-row md:justify-start">
  <Collection
    path={`sensors`}
    query={ref => ref.where('userId', '==', user.uid)
      .where('zoneId', '==', zoneRef.id).where('enabled', '==', true)}
    let:data={sensors}
    let:ref={sensorsRef}
    log>

    {#each sensors as sensor}

      <Collection
        path={'moisture'}
        query={ref => ref.where('userId', '==', user.uid)
          .where('zoneId', '==', zoneRef.id).where('sensorId', '==', sensor.id)
          .orderBy('created')}
        let:data={moistures}
        let:ref={moisturesRef}
        log>

        <div class="flex flex-row justify-center mb-2 bg-white rounded-lg shadow-lg md:mb-0 md:w-5/12 md:mr-2 sensor">
          <div class="w-1/5 p-4 fill-current {iconColor(moistures[moistures.length-1].value, sensor.thresholdValue, sensor.thresholdType) ? 'text-red-400' : 'text-blue-300'}">
            <IoIosWater />
          </div>
          <div class="w-3/5 h-auto py-3 my-auto text-xs leading-snug">
            <div class="font-bold text-left">
              Sensor {zoneDoc.sensors.indexOf(sensor.id)+1}
            </div>
            <div class="text-left">
              Soil Moisture
            </div>
            <div class="text-left">
              {moment(moistures[moistures.length-1].created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
            </div>
          </div>
          <div class="flex w-1/5 text-2xl font-thin text-center align-middle">
            <div class="m-auto">
              {moistures[moistures.length-1].value}%
            </div>
          </div>
        </div>

      </Collection>

    {/each}

  </Collection>
</div>

<style>
  .sensor-icon {
    color: aqua;
  }
  
  /* .sensor:not(:last-child) {
    @apply md:mr-4;
  } */
</style>