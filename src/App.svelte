<script>
  import { FirebaseApp, User, Doc, Collection } from "sveltefire";
  import Zones from "./Zones.svelte"

  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/firestore";
  import "firebase/performance";
  import "firebase/analytics";
  // import "firebase/messaging";
  // import "firebase/storage";

  import Tailwindcss from './Tailwindcss.svelte';

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
</script>

<main>

  {#if !firebaseConfig.projectId}
    <strong>Step 0</strong>
    Create a
    <a href="https://firebase.google.com/">Firebase Project</a>
    and paste your web config into
    <code>App.svelte</code>
    .
  {/if}

  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>

    <!-- 2. 😀 Get the current user -->
    <User let:user let:auth>
      <div class="flex justify-between p-4 bg-green-400">
        <span class="text-2xl font-bold text-white">
          Smart Gardening
        </span>
        <span class="flex justify-end">
          <span class="flex items-center text-sm font-semibold text-gray-100">{user.email}</span>
          <button class="p-1 ml-3 font-semibold text-gray-100 border-2 border-gray-100 shadow-sm hover:bg-red-400" on:click={() => authSignOut()}>Sign Out</button>
        </span>
      </div>

      <Zones {user} />

      <span slot="loading">Loading zones...</span>

      <div slot="signed-out">
        <button class="h-full p-2 font-semibold text-white bg-green-400 hover:bg-green-500" on:click={() => authPopup()}>
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