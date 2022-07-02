<script>
  import { page } from "$app/stores";

  const nav = [
    { title: "Home", path: "/" },
    { title: "About", path: "/about" },
    { title: "Contact", path: "/contact" },
  ];
</script>

<nav class="menu-nav">
  <a href="/" class="enlace">
    <label class="logo">Integriprod</label>
  </a>
  <input type="checkbox" id="check" />
  <label for="check" class="checkbtn">
    <i class="material-icons">menu</i>
  </label>
  <ul class="menu">
    {#each nav as item}
      <li>
        <!-- menu-link -->
        <a
          class="btn ripple"
          class:active={$page.url.pathname === item.path}
          href={item.path}>{item.title}</a
        >
      </li>
    {/each}
  </ul>
</nav>

<style>
  ul.menu {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  .menu li a {
    text-decoration: none;
    -webkit-appearance: button;
    -moz-appearance: button;
    appearance: button;
  }

  .menu-nav {
    background: var(--dark-color);
    height: 50px;
    width: 100%;
    padding: 0 50px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-shadow: 0 5px 5px rgba(0, 0, 0, 0.3);
  }
  .logo {
    height: 40px;
    color: #fff;
    font-size: 35px;
    padding: 0 80px;
    font-weight: bold;
  }
  .menu-nav ul {
    display: flex;
    justify-content: center;
  }
  .menu-nav ul li {
    margin: 0 5px;
  }
  .menu-nav ul li a {
    color: #f3f3f3;
    background-color: transparent;
    font-size: 18px;
    padding: 7px 13px;
    border-radius: 3px;
    text-transform: uppercase;
  }
  .btn.active,
  .btn:hover {
    background-color: var(--secondary-color);
    transition: 0.5s;
  }
  .checkbtn {
    font-size: 30px;
    color: #fff;
    cursor: pointer;
    display: none;
  }
  #check {
    display: none;
  }
  .enlace {
    text-decoration: none;
  }
  @keyframes blinker {
    50% {
      opacity: 0;
    }
  }
  @keyframes hideShow {
    0%,
    16.6%,
    33.33%,
    100% {
      width: 0;
    }
    16.66%,
    33.3% {
      width: auto;
    }
  }

  /* MEDIAS QUERIES */
  @media (max-width: 952px) {
    .menu-nav ul li a {
      font-size: 16px;
    }
    .logo {
      font-size: 30px;
      padding-left: 35px;
    }
  }

  @media (max-width: 858px) {
    .checkbtn {
      display: flex;
    }
    .menu-nav {
      padding: 0 30px;
    }
    .menu-nav ul {
      position: absolute;
      width: 100%;
      height: calc(100vh - 80px);
      background: #2c3e50;
      top: 80px;
      left: -100%;
      text-align: center;
      transition: all 0.5s;
      flex-direction: column;
      z-index: 10;
    }
    .menu-nav ul li {
      margin: 20px 0;
    }
    .menu-nav ul li a {
      font-size: 20px;
    }
    #check:checked ~ ul {
      left: 0;
    }
    .logo {
      font-size: 30px;
      padding-left: 35px;
    }
  }
</style>
