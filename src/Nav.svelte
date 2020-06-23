<script>
  import FaUserCircle from 'svelte-icons/fa/FaUserCircle.svelte'
  import Dialog from '../node_modules/svelma/src/components/Dialog/Dialog.svelte'
  import { Button, Toast} from 'svelma'
  import 'bulma/css/bulma.css'

  function userModal(user) {
    return Dialog.confirm({
      title: 'Logged in as:',
      message: user.email,
      confirmText: 'Logout',
      size: 'is-small'
    })
    .then(result => result && authSignOut())
  }

  export let user;
</script>

<div class="flex flex-row justify-between px-4 py-2">
  <span class="text-xl font-bold text-left text-white">
    Yarden Gnome
  </span>
  <div on:click={() => userModal(user)} class="my-auto text-white cursor-pointer fill-current hover:text-blue-300 usericon">
    <FaUserCircle />
  </div>
</div>

<style>
  * {
    transition: .2s;
  }

  .usericon {
    width: 24px;
    height: 24px;
  }
</style>