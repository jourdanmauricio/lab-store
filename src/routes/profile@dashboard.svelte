<script>
  import { credentials } from "../store/stores";

  const errors = {};

  function resetFieldError() {
    console.log("Clear error");
  }
  function handleSubmit(event) {
    const formData = new FormData(event.target);
    const formUser = {};
    for (const [k, v] of formData.entries()) {
      formUser[k] = v;
    }
    console.log("valor no form", formUser);
    // console.log("valor no store", getUser());
  }
</script>

<section class="personal">
  <h2>{$credentials.email}</h2>
  <button class="btn ripple btn-local">Eliminar cuenta</button>
  <button class="btn ripple btn-local">Cambiar contraseña</button>
</section>

<form on:submit|preventDefault={handleSubmit}>
  <div class="profile-form">
    <div class="group">
      <input
        type="text"
        name="name"
        on:input={resetFieldError}
        value={$credentials.customer.name}
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
        value={$credentials.customer.lastName}
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
      <label for="password">Role</label>
      {#if errors.role}
        <p class="error">
          <small style="color: red"> {errors.role} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="phone"
        on:input={resetFieldError}
        value={$credentials.customer.phone}
        required="required"
      />
      <label for="password">Teléfono</label>
      {#if errors.phone}
        <p class="error">
          <small style="color: red"> {errors.phone} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="phone"
        on:input={resetFieldError}
        value={$credentials.customer.documentType}
        required="required"
      />
      <label for="password">Tipo documento</label>
      {#if errors.documentType}
        <p class="error">
          <small style="color: red"> {errors.documentType} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input
        type="text"
        name="phone"
        on:input={resetFieldError}
        value={$credentials.customer.documentNumber}
        required="required"
      />
      <label for="password">Número documento</label>
      {#if errors.documentNumber}
        <p class="error">
          <small style="color: red"> {errors.documentNumber} </small>
        </p>
      {/if}
    </div>
    <!-- <div class="group">
      <input type="text" name="phone" on:input={resetFieldError} />
      <label for="password">Nueva contraseña</label>
      {#if errors.password}
        <p class="error">
          <small style="color: red"> {errors.password} </small>
        </p>
      {/if}
    </div>
    <div class="group">
      <input type="text" name="phone" on:input={resetFieldError} />
      <label for="password">Confirmación contraseña</label>
      {#if errors.confirmPassword}
        <p class="error">
          <small style="color: red"> {errors.confirmPassword} </small>
        </p>
      {/if}
    </div> -->
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
