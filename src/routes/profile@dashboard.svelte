<script>
  import { credentials, notification } from "../store/stores";
  import { variables } from "$lib/variables";
  import ModalChangePass from "../lib/profile/Modal-change-Pass.svelte";
  import { validateFields } from "./../helpers/validateFileds";

  let errors = {};
  let isLoading = false;

  function resetFieldError(e) {
    delete errors[e.target.name];
    errors = errors;
  }

  async function handleSubmit(event) {
    const formData = new FormData(event.target);
    const formUser = {};
    for (const [key, value] of formData.entries()) {
      formUser[key] = value;
    }
    /* isPassword, isConfirmPassword, isEmail, isName, isPhone, isDocument, none */
    formData.append("name", "isName");
    formData.append("lastName", "isName");
    formData.append("documentType", "isName");
    formData.append("phone", "isPhone");
    formData.append("documentNumber", "isDocument");
    errors = validateFields(formData);

    if (Object.keys(errors).length === 0) {
      isLoading = true;
      let url, method;
      try {
        if ($credentials.customer) {
          url = `${variables.basePath}/customers/${$credentials.customer.id}`;
          method = "PATCH";
        } else {
          url = `${variables.basePath}/customers`;
          method = "POST";
        }

        formUser.userId = $credentials.id;
        delete formUser.role;

        const res = await fetch(url, {
          method: method,
          body: JSON.stringify(formUser),
          headers: {
            "content-type": "application/json",
            Authorization: `Bearer ${$credentials.token}`,
          },
        });
        const data = await res.json();
        if (res.status === 201) {
          const user = $credentials;
          user.customer = data;
          localStorage.setItem("user", JSON.stringify(user));
          credentials.setCredentials(user);
          notification.show("Perfil actualizado correctamente 👌", "success");
          // goto("/dashboard");
        } else {
          let message = "";
          message = res.statusText
            ? `${res.status}: ${res.statusText}`
            : "Error actualizando el perfil 😞";
          throw message;
        }
      } catch (err) {
        notification.show(err, "error");
      } finally {
        isLoading = false;
      }
    }
  }
</script>

<!-- <section class="personal">
  <h2>{$credentials.email}</h2>
  <button class="btn ripple btn-local">Eliminar cuenta</button>

  <button class="btn ripple btn-local" on:click={() => getModal().open()}
    >Cambiar contraseña</button
  >
</section> -->

<h2>{$credentials.email}</h2>

<ModalChangePass />

<form on:submit|preventDefault={handleSubmit}>
  <div class="profile-form">
    <div class="group">
      <input
        type="text"
        name="name"
        on:input={resetFieldError}
        value={$credentials.customer ? $credentials.customer.name : ""}
        required="required"
      />
      <label for="password">Nombre</label>
      {#if errors.name}
        <p class="error">
          <small style="color: red"> {errors.name} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="lastName"
        on:input={resetFieldError}
        value={$credentials.customer ? $credentials.customer.lastName : ""}
        required="required"
      />
      <label for="password">Apellido</label>
      {#if errors.lastName}
        <p class="error">
          <small style="color: red"> {errors.lastName} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="role"
        on:input={resetFieldError}
        value={$credentials.role}
        readonly
      />
      <!-- required="required" -->
      <label for="role">Role</label>
    </div>
    <div class="group">
      <input
        type="text"
        name="phone"
        on:input={resetFieldError}
        value={$credentials.customer ? $credentials.customer.phone : ""}
        required="required"
      />
      <label for="phone">Teléfono</label>
      {#if errors.phone}
        <p class="error">
          <small style="color: red"> {errors.phone} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="documentType"
        on:input={resetFieldError}
        value={$credentials.customer ? $credentials.customer.documentType : ""}
        required="required"
      />
      <label for="documentType">Tipo documento</label>
      {#if errors.documentType}
        <p class="error">
          <small style="color: red"> {errors.documentType} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="documentNumber"
        on:input={resetFieldError}
        value={$credentials.customer
          ? $credentials.customer.documentNumber
          : ""}
        required="required"
      />
      <label for="documentNumber">Número documento</label>
      {#if errors.documentNumber}
        <p class="error">
          <small style="color: red"> {errors.documentNumber} </small>
        </p>
      {/if}
    </div>
  </div>
  <div class="controls">
    <button type="submit" class="btn ripple btn-local">Enviar</button>
  </div>
</form>

<style>
  .personal {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
  }
  .controls {
    text-align: right;
  }
  .btn {
    padding: 5px 40px;
    margin-top: 30px;
  }
  .profile-form {
    display: grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 30px;
  }
  .error {
    position: absolute;
    top: 45px;
    right: 15px;
  }

  input {
    margin-top: 35px;
    background: none;
    color: var(--principal-color);
    font-size: 16px;
    padding: 5px 5px 5px 15px;
    display: block;
    width: 100%;
    border: none;
    border-radius: 10px;
    border: 1px solid #141e30;
  }
  input:-webkit-autofill,
  input:-webkit-autofill:hover,
  input:-webkit-autofill:focus {
    -webkit-text-fill-color: #141e30;
    transition: background-color 5000s ease-in-out 0s;
    caret-color: var(--principal-color);
  }

  label {
    /* color: var(--text-white-color); */
    font-size: 15px;
    font-weight: normal;
    position: absolute;
    pointer-events: none;
    left: 10px;
    top: 40px;
    transition: 300ms ease all;
    padding: 0 2px;
  }
  input:focus ~ label,
  input:read-only ~ label,
  input:valid ~ label {
    top: 18px;
    font-size: 12px;
    left: 11px;
    background: none;
  }
  .group {
    position: relative;
  }
</style>
