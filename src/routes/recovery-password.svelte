<script>
  import { notification } from "../store/stores.js";
  import { page } from "$app/stores";
  import { goto } from "$app/navigation";
  import Spinner from "../lib/Spinner.svelte";
  let password = null;
  let confirmPassword = null;
  let errors = {};
  let isLoading = false;
  const token = $page.url.searchParams.get("token");

  function resetFieldError() {
    errors = {};
  }

  async function submitHandler() {
    if (password.length < 8) {
      errors.password = "El password debe contener al menos 8 caracteres";
    }
    if (password !== confirmPassword) {
      errors.confirmPassword = "La confirmación debe ser igual a la contraseña";
    }
    if (Object.keys(errors).length === 0) {
      isLoading = true;
      try {
        isLoading = true;
        const res = await fetch(
          "http://localhost:3100/api/v1/auth/change-password",
          {
            method: "POST",
            body: JSON.stringify({
              token,
              newPassword: password,
            }),
            headers: {
              "content-type": "application/json",
            },
          }
        );
        const data = await res.json();
        if (res.status === 200) {
          notification.show("Password changed", "info");
          goto("/");
        } else {
          throw "Error al cambiar el password";
        }
      } catch (err) {
        notification.show(err, "error");
      } finally {
        isLoading = false;
      }
    }
  }
</script>

<div class="recovery-box">
  <h2>Cambio de contraseña</h2>

  <form class="recovery-form" on:submit|preventDefault={submitHandler}>
    <div class="group">
      <input
        type="password"
        name="password"
        bind:value={password}
        on:input={resetFieldError}
        required="required"
      />
      <label for="password">Nueva contraseña</label>
      {#if errors.password}
        <p class="error">
          <small style="color: red"> {errors.password} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="password"
        name="confirmPassword"
        bind:value={confirmPassword}
        on:input={resetFieldError}
        required="required"
      />
      <label for="password">Repita la contraseña</label>
      {#if errors.confirmPassword}
        <p class="error">
          <small style="color: red"> {errors.confirmPassword} </small>
        </p>
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
  input[type="password"] {
    letter-spacing: 0.3em;
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
