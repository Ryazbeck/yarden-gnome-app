<script>
  import { Doc } from "sveltefire";
  import IoIosAdd from 'svelte-icons/io/IoIosAdd.svelte'

  export let zone;
  let zoneNameUpdate;
  let editName = false;
  let notes = zone.ref.collection("notes").get()
  console.log(notes)

  let zoneDescriptionUpdate;
  let editDescription = false;
</script>

<div>
  <div>
    <h3>Notes</h3>
  </div>
  <div>
    <!-- <span on:click={() => editName=true} class="ml-2 cursor-pointer icon"> -->
    <IoIosAdd />
    Add Note 
  </div>
  {console.log(zone)}
</div>