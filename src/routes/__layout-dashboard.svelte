<script>
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import { isLogged, credentials, notification } from "../store/stores.js";
  import Footer from "../lib/Footer.svelte";
  import Snackbar from "../lib/Snackbar.svelte";
  import NavDashboard from "../lib/Nav-dashboard.svelte";
  import SidebarDashboard from "../lib/Sidebar-dashboard.svelte";

  let fullWidth = true;

  const fullDisplay = () => (fullWidth = !fullWidth);

  onMount(async () => {
    console.log("Mount Dashboard");
    if (!$isLogged) {
      const user = localStorage.getItem("user");
      if (user) {
        console.log("user", JSON.parse(user));
        isLogged.login();
        credentials.setCredentials(JSON.parse(user));
      } else {
        goto("/");
      }
    }
  });
</script>

<div class={fullWidth ? "full-container" : "container"}>
  <header class={fullWidth ? "full-header" : "header"}>
    <NavDashboard {fullDisplay} />
  </header>

  <Snackbar />

  <aside class={fullWidth ? "full-panel" : "panel"}>
    <SidebarDashboard />
  </aside>

  <main class={fullWidth ? "full-main" : "main"}>
    <section class="section">
      <slot />
    </section>
  </main>
  <footer class={fullWidth ? "full-footer" : "footer"}>
    <Footer />
  </footer>
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    height: 100vh;
    background-color: var(--dark-color);
  }
  .header {
    position: sticky;
    z-index: 997;
    top: 0;
  }

  .section {
    padding: 0 2rem;
    margin-left: auto;
    margin-right: auto;
    max-width: 1200px;
    text-align: center;
  }
  .panel {
    position: fixed;
    z-index: 998;
    top: 3rem;
    bottom: 0;
    left: 0;
    right: 0;
    width: 15%;
    overflow-y: auto;
    opacity: 0;
    visibility: hidden;
    transition: transform 0.5s ease, opacity 0.5s ease;
    background-color: var(--dark-color);
    /* De arriba */
    /* transform: translate(0, -100%); */
    /* De abajo */
    /* transform: translate(0, 100%); */
    /* De la derecha */
    /* transform: translate(100%, 0); */
    /* De la izquierda */
    transform: translate(-100%, 0);
  }

  .main {
    flex-grow: 1;
    background-color: var(--text-white-color);
  }

  .full-container {
    background-color: var(--dark-color);
    height: 100vh;
    display: grid;
    grid-template-columns: 15% 85%;
    grid-template-rows: 50px auto 35px;
    grid-template-areas:
      "header header"
      "panel main"
      "footer footer";
  }

  .full-header {
    grid-area: header;
    z-index: 997;
  }

  .full-main {
    grid-area: main;
    background-color: var(--text-white-color);
  }

  .full-panel {
    height: 100%;
    background-color: var(--dark-color);
    grid-area: panel;
    align-self: start;
    position: sticky;
    top: 3rem;
    justify-content: flex-start;
    visibility: visible;
    overflow-y: auto;
    /* De la izquierda  */
    transition: transform 0.5s ease, opacity 0.5s ease;
    transform: translate(0, 0);
  }

  .full-footer {
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
