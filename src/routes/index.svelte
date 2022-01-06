<script>
    import '../app.css'
    import authStore from '$lib/authStore';
 import {
  auth, 
  db} from '$lib/firebase'
  import {
  GoogleAuthProvider,
  signInWithPopup,
  signOut
} from 'firebase/auth'
import { goto } from '$app/navigation';
import { onDestroy, onMount } from 'svelte';



const sub = authStore.subscribe(async (info) => {
    if (info.isLoggedIn) {
      await goto('/');
    }
  });
  
 onDestroy(() => {
    sub();
  });
 onMount(() => {
      auth.onAuthStateChanged((user) => {
        authStore.set({
          isLoggedIn: user !== null,
          user,
          firebaseControlled: true,
        });
      });
  });

const login = async()=>{
    try {
        const provider = new GoogleAuthProvider()
      await signInWithPopup(auth,provider)
    } catch (e) {
      console.log(e);
    }
}
const logout = async()=>{
    try {
        await signOut(auth)
    } catch (e) {
      console.log(e);
}}


let search = "";





</script>



<div class="border-gray-500 font-bold flex flex-col mb-2 container items-center justify-items-center">
<h1 class="text-rose-500">Welcome to SvelteKit</h1>


<p>Visit kit.svelte.dev to read the documentation</p>


{#if $authStore.firebaseControlled}
 {#if !$authStore.isLoggedIn}
 <button class="px-4 py-2 rounded-md text-sm font-medium border focus:outline-none focus:ring transition text-blue-600 border-blue-600 hover:text-white hover:bg-blue-600 active:bg-blue-700 focus:ring-blue-300" on:click={login}>로그인</button>
 {:else}
 <button class="px-4 py-2 rounded-md text-sm font-medium border focus:outline-none focus:ring transition text-blue-600 border-blue-600 hover:text-white hover:bg-blue-600 active:bg-blue-700 focus:ring-blue-300" on:click={logout}>로그아웃</button>
 {/if}
{/if}

</div>





<div class="shadow flex">
    <input class="w-full rounded p-2" type="text" placeholder="Search..." bind:value={search}>
    <button class="w-auto flex justify-end items-center text-blue p-2 hover:text-blue-light">
        <!-- <i class="material-icons">search</i> -->
    </button>
</div>