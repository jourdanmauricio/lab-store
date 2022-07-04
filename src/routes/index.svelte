<script>
  import { isLogged, credentials, notification } from "../store/stores.js";
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  onMount(() => {
    console.log("Mount index");
  });

  let errors = {};
  // let email = null;
  let email = "jourdanmauricio@gmail.com";
  // let password = null;
  let password = "mau10ti0";
  let isLoading = false;

  // function isRequired(value) {
  //   return value != null && value !== "";
  // }

  // function isNameValid(value) {
  //   return /^[a-zA-Z]$/.test(value);
  // }

  function isEmailValid(value) {
    return /^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/.test(value);
  }

  function isPassValid(value) {
    return value.trim().length > 7;
  }

  async function submitHandler(event) {
    const formData = new FormData(event.target);

    for (let field of formData) {
      const [key, value] = field;

      if (key === "email") {
        if (!isEmailValid(value)) {
          errors[key] = "Email inválido";
        }
      }
      if (key === "password")
        if (!isPassValid(value)) {
          errors[key] = "El password debe contener al menos 8 caracteres";
        }
    }

    if (Object.keys(errors).length === 0) {
      isLoading = true;
      try {
        const res = await fetch("http://localhost:3100/api/v1/auth/login", {
          method: "POST",
          body: JSON.stringify({
            email,
            password,
          }),
          headers: {
            "content-type": "application/json",
          },
        });
        const data = await res.json();
        if (res.status === 200) {
          // Almaceno token en localstorage
          localStorage.setItem("token", data.token);
          localStorage.setItem("user", JSON.stringify(data.user));
          isLogged.login();
          credentials.setCredentials(data.user);
          console.log("user store", $credentials);
          notification.show("Bienvenido!!!", "info");
          goto("/dashboard");
        } else {
          throw "Error en Login";
        }
      } catch (err) {
        notification.show(err, "error");
      } finally {
        isLoading = false;
      }
    }
  }

  function resetFieldError(e) {
    delete errors[e.target.name];
    errors = errors;
  }
</script>

<div class="login-box">
  <h2>Login</h2>
  <form class="login-form" on:submit|preventDefault={submitHandler}>
    <div class="group">
      <input
        type="text"
        name="email"
        bind:value={email}
        on:input={resetFieldError}
        required="required"
      />

      <label for="email">Email</label>
      {#if errors.email}
        <p class="error"><small style="color: red"> {errors.email} </small></p>
      {/if}
    </div>
    <div class="group">
      <input
        type="password"
        name="password"
        bind:value={password}
        on:input={resetFieldError}
        required="required"
      />
      <label for="password">Password</label>
      {#if errors.password}
        <p class="error">
          <small style="color: red"> {errors.password} </small>
        </p>
      {/if}
    </div>

    <p><a href="/forgot-password"> ¿Olvidó su password?</a></p>

    <button class="btn ripple btn-local">Login</button>
  </form>
</div>

<style>
  .error {
    position: absolute;
    top: 17px;
  }
  .login-box {
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
  .login-box h2 {
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
  input[type="password"] {
    letter-spacing: 0.3em;
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

  a {
    text-decoration: none;
    color: var(--text-white-color);
  }
  a:link,
  a:visited,
  a:focus,
  a:hover,
  a:active {
    color: var(--text-white-color);
  }

  @media (max-width: 858px) {
    .login-box {
      width: 90%;
      padding: 20px;
    }
  }
</style>
