<script>
  import { page } from "$app/stores";
  import { credentials } from "../store/stores.js";

  // const nav = [
  //   { title: "Perfil", path: "/profile", role="" },
  //   { title: "About", path: "/about" },
  //   { title: "Contact", path: "/contact" },
  // ];
</script>

<nav>
  <ul class="menu">
    <li>
      <a
        class="btn"
        class:active={$page.url.pathname === "/profile"}
        href="/profile"
        ><i class="material-icons">person</i><span class="text-link"
          >Perfil</span
        ></a
      >
    </li>
    <li>
      <a
        class="btn"
        class:active={$page.url.pathname === "/products"}
        href="/products"
        ><i class="material-icons" style="top: 30px;">stars</i><span
          class="text-link">Productos</span
        ></a
      >
    </li>
  </ul>
</nav>

<style>
  .text-link {
    margin-left: 5px;
  }
  ul.menu {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  .menu {
    color: var(--text-white-color);
    display: flex;
    flex-direction: column;
  }

  .menu li a {
    display: flex;
    margin-top: 15px;
    align-items: center;
    width: 100%;
    color: var(--text-white-color);
    text-decoration: none;
    -webkit-appearance: button;
    -moz-appearance: button;
    appearance: button;
    background-color: transparent;
    font-size: 16px;
    /* padding: 7px 13px; */
    border-radius: 3px;
    /* text-transform: uppercase; */
  }
  .btn.active,
  .btn:hover {
    background-color: var(--secondary-color);
    transition: 0.5s;
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
</style>
