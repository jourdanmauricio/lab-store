<script>
  import NavHome from "$lib/Nav-home.svelte";
  import Footer from "../lib/Footer.svelte";
</script>

<div class="container">
  <header>
    <NavHome />
  </header>

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
</style>
