<script>
  import NavHome from "$lib/Nav-home.svelte";
  import Footer from "../lib/Footer.svelte";
  import Snackbar from "../lib/Snackbar.svelte";
</script>

<div class="container">
  <header>
    <NavHome />
  </header>
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
