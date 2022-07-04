<script>
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import {
    isLogged,
    credentials,
    notification,
  } from "../src/src/store/stores.js";
  import Footer from "../src/src/lib/Footer.svelte";
  import Snackbar from "../src/src/lib/Snackbar.svelte";
  import NavDashboard from "../src/src/lib/Nav-dashboard.svelte";
  import SidebarDashboard from "../src/src/lib/Sidebar-dashboard.svelte";

  let fullWidth = true;

  function fullDisplay() {
    fullWidth = !fullWidth;
  }
  onMount(async () => {
    console.log("Mount Dashboard");
    if (!$isLogged) {
      const token = localStorage.getItem("token");
      if (token) {
        const user = await localStorage.getItem("user");
        console.log("user", JSON.parse(user));
        isLogged.login();
        credentials.setCredentials(JSON.parse(user));
      } else {
        goto("/");
      }
    }
  });
</script>

<!-- <div class="container"> -->
<div class={fullWidth ? "clean-display" : "container"}>
  <header class="header">
    <NavDashboard />
  </header>

  <Snackbar />

  <!-- <aside class="sidebar"> -->
  <aside class={fullWidth ? "cleanSidebar" : "sidebar"}>
    <SidebarDashboard />
  </aside>

  <main class="content">
    <slot />
    <button on:click={fullDisplay}>Full</button>
  </main>
  <footer class="footer">
    <Footer />
  </footer>
</div>

<style>
  .container {
    height: 100vh;
    display: grid;
    grid-template-columns: 150px 1fr;
    grid-template-rows: 50px 1fr 35px;
    grid-template-areas:
      "header header"
      "sidebar content "
      "footer footer";
  }
  .clean-display {
    height: 100vh;
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 50px 1fr 35px;
    grid-template-areas:
      "header"
      "content "
      "footer";
  }

  .header {
    grid-area: header;
  }
  .content {
    grid-area: content;
    background-color: #fff;
    padding: 0 1rem;
  }
  .sidebar {
    grid-area: sidebar;
    background-color: var(--dark-color);
  }
  .cleanSidebar {
    display: none;
  }

  .footer {
    grid-area: footer;
  }
  :global(.ripple:active:after) {
    transform: scale(0, 0);
    opacity: 0.3;
    transition: 0s;
  }
  :global(:root) {
    --principal-color: #141e30;
    --secondary-color: #243b55;
    --dark-color: rgba(0, 0, 0, 0.5);
    --text-white-color: #c6c6c6;
  }
  :global(.btn) {
    border-radius: 5px;
    background-color: var(--secondary-color);
    color: #fff;
    font-size: 18px;
    padding: 3px 15px;
  }
  :global(.ripple) {
    position: relative;
    overflow: hidden;
    transform: translate3d(0, 0, 0);
  }
  :global(.ripple:after) {
    content: "";
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    pointer-events: none;
    background-image: radial-gradient(circle, #fff 10%, transparent 10.01%);
    background-repeat: no-repeat;
    background-position: 50%;
    transform: scale(10, 10);
    opacity: 0;
    transition: transform 0.5s, opacity 1s;
  }
</style>
