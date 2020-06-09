<script>
  import { Doc } from "sveltefire";
  import moment from 'moment';
  import MdAssignment from 'svelte-icons/md/MdAssignment.svelte'
	import { flip } from 'svelte/animate';
	import { quintOut } from 'svelte/easing';

  export let note;
  let expandNote = false;
</script>


<div class="note-container flex flex-row w-full h-16 mb-3 text-left bg-white border-t-2 border-green-300 rounded-sm cursor-pointer {expandNote ? 'expanded' : 'hover:bg-green-100'}">
  <div class="w-1/12 h-full p-1 pt-3 mx-1">
    <span class="icon">
      <MdAssignment />
    </span>
  </div>

  <div class="flex flex-col justify-between w-11/12 h-full pt-2 note" on:click={() => expandNote = expandNote ? false : true}>
    <div class="h-4 mb-1 text-xs text-gray-600">
      {moment(note.created.toDate()).format("MMMM Do YYYY, h:mm:ss a")}
    </div>
    <div class="relative z-0 h-full pr-4 overflow-hidden text-sm leading-4 {expandNote ? 'expanded-note' : ''}">
      {note.note}
    </div>
  </div>
</div>

<style>
  .icon {
    color: #68D391;
    width: 16px;
    height: 16px;
  }

  .note {
    position: relative;
  }

  .note:after {
    content: "";
    position: absolute;
    top: 0; bottom: 0; left: 0; right:0;
    z-index: 20;
    -webkit-box-shadow: inset 0px -20px 8px -10px rgba(255,255,255,1);
    -moz-box-shadow: inset 0px -20px 8px -10px rgba(255,255,255,1);
    box-shadow: inset 0px -20px 8px -10px rgba(255,255,255,1);
  }

  .expanded {
    @apply h-full;
  }

  .expanded-note {
    @apply pb-3;
  }
</style>