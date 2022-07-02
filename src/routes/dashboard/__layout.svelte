<div class="container">
  <nav>
    <ul>
      <li><a href="/">Inicio</a></li>
      <li><a href="/dashboard">Dashboard</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>
  <slot />
</div>

<style>
  .container {
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
