<script>
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import { isLogged, credentials, notification } from "./../stores.js";
  import Footer from "../lib/Footer.svelte";
  import Snackbar from "../lib/Snackbar.svelte";

  onMount(() => {
    if (!$isLogged) goto("/");
  });
</script>

<div class="container">
  <nav>
    <ul>
      <li><a href="/">Inicio</a></li>
      <li><a href="/dashboard">Dashboard</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/contact">Contact</a></li>
      <li>
        <i class="material-icons" style="color: yellow;">notifications_filled</i
        >
      </li>
      <li><i class="material-icons" style="color: red;">person_filled</i></li>
    </ul>
  </nav>
  <Snackbar />
  <main>
    <slot />
  </main>
  <Footer />
</div>

<style>
  main {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    background: linear-gradient(#141e30, #243b55);
  }

  ul {
    display: flex;
    list-style: none;
  }
  li {
    margin-right: 10px;
  }
</style>
