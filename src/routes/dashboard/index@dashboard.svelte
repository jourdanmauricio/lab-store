<script>
  import { isLogged, credentials } from "./../../stores";
  import { goto } from "$app/navigation";

  function logout() {
    localStorage.removeItem("token");
    isLogged.logout();
    console.log("isLogged", $isLogged);
    credentials.setCredentials({});
    console.log("credentials", $credentials);
    goto("/");
  }
</script>

<h1>Dashborad</h1>
<button on:click={logout}>Logout</button>
