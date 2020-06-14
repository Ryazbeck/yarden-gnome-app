<script>
  import { FirebaseApp, User, Doc, Collection } from "sveltefire";
  import Dashboard from "./Dashboard.svelte"

  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/firestore";
  import "firebase/performance";
  import "firebase/analytics";
  // import "firebase/messaging";
  // import "firebase/storage";

  import FaUserCircle from 'svelte-icons/fa/FaUserCircle.svelte'
  import { Button, Dialog, Toast } from 'svelma'

  import Tailwindcss from './Tailwindcss.svelte';
  import 'bulma/css/bulma.css'

  let firebaseConfig = {
    apiKey: "AIzaSyCzha0sFqdty4ildyFuyR4qVCY4kjRel_w",
    authDomain: "smart-gardening-2a1fe.firebaseapp.com",
    databaseURL: "https://smart-gardening-2a1fe.firebaseio.com",
    projectId: "smart-gardening-2a1fe",
    storageBucket: "smart-gardening-2a1fe.appspot.com",
    messagingSenderId: "867533415028",
    appId: "1:867533415028:web:28b243b76f3bdb1c1e6721",
    measurementId: "G-V6R8ZQT946"
  };

  firebase.initializeApp(firebaseConfig);

  let userId;

  var provider = new firebase.auth.GoogleAuthProvider();
  function authPopup() {
      firebase.auth().signInWithPopup(provider).then(function(result) {
      // This gives you a Google Access Token. You can use it to access the Google API.
      var token = result.credential.accessToken;
      // The signed-in user info.
      userId = result.user;
      // ...
    }).catch(function(error) {
      // Handle Errors here.
      var errorCode = error.code;
      var errorMessage = error.message;
      // The email of the user's account used.
      var email = error.email;
      // The firebase.auth.AuthCredential type that was used.
      var credential = error.credential;
      // ...
    });
  }

  function authSignOut() {
    firebase.auth().signOut().then(function() {
      // Sign-out successful.
    }).catch(function(error) {
      // An error happened.
    });
  }

  function userModal(user) {
    console.log(user)
    return Dialog.confirm({
      title: 'Logged in as:',
      message: user.email,
      confirmText: 'Logout',
      size: 'is-small'
    })
    .then(result => result && authSignOut())
  }
</script>

<main>

  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>

    <!-- 2. 😀 Get the current user -->
    <User let:user let:auth>
      <div class="flex flex-row justify-between px-4 py-2">
        <span class="text-xl font-bold text-left text-white">
          Yarden Gnome
        </span>
        <span on:click={() => userModal(user)} class="my-auto icon">
          <FaUserCircle />
          <!-- <span class="flex items-center justify-center text-xs font-semibold text-gray-100 md:text-sm">{user.email}</span>
          <button class="w-16 p-1 mx-auto mt-1 text-xs font-semibold text-gray-100 bg-green-500 shadow-sm md:ml-3 md:w-auto hover:bg-red-400" on:click={() => authSignOut()}>Sign Out</button> -->
        </span>
      </div>

      <Dashboard {user} />

      <!-- <Zones {user} /> -->

      <!-- <div class="h-10 max-w-screen-md mx-auto font-bold text-center text-white align-middle bg-green-200 border-2 border-b-0 border-green-400 rounded-t">
      </div> -->

      <div slot="signed-out">
        <button class="h-full p-2 font-semibold text-white bg-green-500 hover:bg-green-600" on:click={() => authPopup()}>
          Sign In With Google
        </button>
      </div>

    </User>
  </FirebaseApp>

  <Tailwindcss />
</main>


<!-- Styles -->
<style>
  * {
    transition: .2s;
  }
  
  main {
    text-align: center;
    padding: 0;
    margin: 0 auto;
  }

  h1,
  em {
    color: #ff3e00;
  }

  .icon {
    color: white;
  }

  hr {
    height: 1px;
    border: none;
    background: rgb(195, 195, 195);
  }

  @media (min-width: 640px) {
    main {
      /* max-width: none; */
    }
  }
</style>