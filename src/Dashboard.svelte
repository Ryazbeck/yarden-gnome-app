<script>
  import { Doc, Collection } from "sveltefire";
  import Alerts from './Alerts.svelte'
  import Sensors from './Sensors.svelte'
  import Charts from './Charts.svelte'
  import Notes from './Notes.svelte'
  import "../node_modules/bulma/css/bulma.css";
  import '@fortawesome/fontawesome-free/css/all.css'
  import TiUser from 'svelte-icons/ti/TiUser.svelte'

  export let user;
</script>

<div class="container max-w-screen-md p-3 mx-auto">


  <div class="md:flex md:flex-col md:justify-start">
    <Collection
      path={`sensors`}
      query={ref => ref.where('userId', '==', user.uid).where('enabled', '==', true)}
      let:data={sensors}
      let:ref={sensorsRef}
      log>

      <Sensors {user} {sensors} />

      <Charts {user} {sensors} />

    </Collection>
  </div>

  <Notes {user} />

</div>