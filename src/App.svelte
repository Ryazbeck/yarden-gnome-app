<script>
  
  import { FirebaseApp, User, Doc, Collection } from "sveltefire";
  import Nav from "./Nav.svelte"
  import Dashboard from "./Dashboard.svelte"

  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/firestore";
  import "firebase/performance";
  import "firebase/analytics";
  // import "firebase/messaging";
  // import "firebase/storage";

  import Tailwindcss from './Tailwindcss.svelte';
  import '@fortawesome/fontawesome-free/css/all.css'

  const firebaseConfig = {
    apiKey: "AIzaSyBeuvFfpE-R2t_hjM533s0mBq0D6E4JXz0",
    authDomain: "yardengnome.firebaseapp.com",
    databaseURL: "https://yardengnome.firebaseio.com",
    projectId: "yardengnome",
    storageBucket: "yardengnome.appspot.com",
    messagingSenderId: "719452905990",
    appId: "1:719452905990:web:c18875be7253106854d987",
    measurementId: "G-7EGCYS2RSP"
  };

  firebase.initializeApp(firebaseConfig);

  let userId;
  let sessionStorage;

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

  <!-- Firebase App -->
  <FirebaseApp {firebase}>

    <!-- Get the current user -->
    <User persist={sessionStorage} let:user let:auth>

      <Nav {user} />

      <Dashboard {user} />

      <div class="flex flex-col justify-center h-screen" slot="signed-out">
        <button class="p-2 mx-auto font-semibold text-white bg-green-500 button hover:bg-blue-300" on:click={() => authPopup()}>
          Sign In with Google
        </button>
      </div>

    </User>
  </FirebaseApp>
  
  <Tailwindcss />

</main>


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