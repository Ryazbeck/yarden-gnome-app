<script>
  import moment from 'moment';
  import { Doc, Collection } from "sveltefire";

  import Sensor from './Sensor.svelte'
  import Charts from './Charts.svelte'

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
      query={ref => ref.where('created', '>', moment().subtract(4,'h').toDate()).where('userId', '==', user.uid)
        .where('zoneId', '==', zoneRef.id).orderBy('created')}
      let:data={moistures}
      let:ref={moisturesRef}
      log>

      <div class="flex flex-col justify-start mb-4 md:flex-row md:justify-between">
        {#each sensors as sensor}
          <Sensor {sensor} {moistures} />
        {/each}
      </div>
      
      <div class="mb-4">
        <Charts {sensors} {moistures} />
      </div>

    </Collection>


  </Collection>
</div>