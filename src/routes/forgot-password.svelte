<script>
  import { notification } from "./../stores.js";
  import { goto } from "$app/navigation";
  import Spinner from "../lib/Spinner.svelte";
  let email = null;
  let error = "";
  let isLoading = false;

  async function submitHandler() {
    if (!/^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/.test(email)) {
      error = "Email inválido";
    }
    if (error.length === 0) {
      try {
        isLoading = true;
        const res = await fetch("http://localhost:3100/api/v1/auth/recovery", {
          method: "POST",
          body: JSON.stringify({
            email,
          }),
          headers: {
            "content-type": "application/json",
          },
        });
        const data = await res.json();
        if (res.status === 200) {
          isLoading = false;
          notification.show("Email enviado", "info");
          goto("/");
        } else {
          throw "Error al recuperar password";
        }
      } catch (err) {
        isLoading = false;
        notification.show(err, "error");
      }
    }
  }
</script>

<div class="recovery-box">
  <h2>Recuperar contraseña</h2>

  <form class="recovery-form" on:submit|preventDefault={submitHandler}>
    <div class="group">
      <input
        type="text"
        name="email"
        bind:value={email}
        on:input={() => (error = "")}
      />

      <label for="email">Email</label>
      {#if error.length > 0}
        <p class="error"><small style="color: red"> {error} </small></p>
      {/if}
    </div>

    <button class="btn ripple btn-local">Enviar</button>
  </form>
  {#if isLoading}
    <Spinner />
  {/if}
</div>

<style>
  .error {
    position: absolute;
    top: 17px;
  }
  .recovery-box {
    text-align: center;
    margin-left: auto;
    margin-right: auto;
    width: 50%;
    min-width: 280px;
    max-width: 550px;
    padding: 40px;
    background: rgba(0, 0, 0, 0.5);
    box-sizing: border-box;
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
    border-radius: 10px;
  }
  .recovery-box h2 {
    margin: 0 0 30px;
    padding: 0;
    color: #fff;
    text-align: center;
  }
  input {
    margin-top: 35px;
    background: none;
    color: var(--text-white-color);
    font-size: 18px;
    padding: 5px 5px 5px 15px;
    display: block;
    width: 100%;
    border: none;
    border-radius: 10px;
    border: 1px solid var(--text-white-color);
  }
  input:-webkit-autofill,
  input:-webkit-autofill:hover,
  input:-webkit-autofill:focus {
    -webkit-text-fill-color: var(--text-white-color);
    transition: background-color 5000s ease-in-out 0s;
    caret-color: var(--text-white-color);
  }
  input:focus ~ label,
  input:valid ~ label {
    top: -15px;
    font-size: 12px;
    left: 11px;
    background: none;
  }
  .group {
    position: relative;
  }
  label {
    color: var(--text-white-color);
    font-size: 16px;
    font-weight: normal;
    position: absolute;
    pointer-events: none;
    left: 10px;
    top: 8px;
    transition: 300ms ease all;
    padding: 0 2px;
  }
  .btn-local {
    margin-top: 40px;
  }

  p {
    text-align: right;
    cursor: pointer;
  }

  @media (max-width: 858px) {
    .recovery-box {
      width: 90%;
      padding: 20px;
    }
  }
</style>
