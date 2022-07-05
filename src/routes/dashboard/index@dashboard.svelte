<script>
  import { isLogged, credentials } from "../../store/stores";
  import { goto } from "$app/navigation";

  function logout() {
    //localStorage.removeItem("token");
    localStorage.removeItem("user");
    isLogged.logout();
    credentials.setCredentials({});
    goto("/");
  }
</script>

<h1>Dashborad</h1>
<button on:click={logout}>Logout</button>
