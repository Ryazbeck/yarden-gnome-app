<script>
  import firebase from "firebase/app";
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
    <Sensors {zoneDoc} {zoneRef} {user} />
    <Charts {zoneDoc} />
    <Notes {zoneRef}  />

  </Doc>
</div>