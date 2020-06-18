<script>
  import moment from 'moment';
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

  export let sensor;
  export let moistures;
  let sensorMoistures = moistures.filter(moisture => moisture.sensorId == sensor.ref.id)
  let lastMoisture = sensorMoistures[sensorMoistures.length-1];
</script>

<div class="flex flex-row justify-center mb-0 bg-white rounded-lg shadow-lg md:w-5/12 sensor">
    <div class="w-1/5 p-4 fill-current {iconColor[sensor.sensorType](lastMoisture.value, sensor)}">
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
        {moment(lastMoisture.created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
    </div>
    </div>
    <div class="flex w-1/5 text-2xl font-thin text-center align-middle">
    <div class="m-auto">
        {lastMoisture.value}%
    </div>
    </div>
</div>

<style>
  .sensor-icon {
    color: aqua;
  }

  @media (max-width: 767px) {
    .sensor {
      @apply mb-2 !important;
    }
  }

  @media (min-width: 768px) {
    .sensor:not(:last-child) {
      @apply mr-2;
    }
  }
</style>