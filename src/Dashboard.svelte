<script>
  import { Doc } from "sveltefire";
  import Alerts from './Alerts.svelte'
  import Sensors from './Sensors.svelte'
  import Charts from './Charts.svelte'
  import Notes from './Notes.svelte'

  export let user;
</script>



<div class="container max-w-screen-md p-3 mx-auto">
  <Doc 
    path={`zones/o7xjEbtq1Km1dcNpo2ys`} 
    let:data={zoneDoc} 
    let:ref={zoneRef}
    >

    <!-- <Alerts {zoneDoc} {user} /> -->
    <Sensors {zoneRef} {user} />
    <Notes {zoneRef}  />

  </Doc>
</div>