<script>
  import { credentials, notification } from "./../../store/stores";
  import { onMount } from "svelte";
  import Modal, { getModal } from "./../Modal.svelte";
  import { variables } from "$lib/variables";
  import { validateFields } from "./../../helpers/validateFileds";
  import Spinner from "../Spinner.svelte";

  let openModal = false;
  let isLoading = false;
  let errors = {};
  let oldPassword = "";
  let newPassword = "";
  let confirmPassword = "";

  function resetFieldError(e) {
    delete errors[e.target.name];
    errors = errors;
  }

  async function formHandle(event) {
    const formData = new FormData(event.target);
    /* isPassword, isConfirmPassword, isEmail, isName, isPhone, isDocument, none */
    formData.append("oldPassword", "isPassword");
    formData.append("newPassword", "isPassword");
    formData.append("confirmPassword", "isConfirmPassword");
    errors = validateFields(formData);

    if (Object.keys(errors).length === 0) {
      isLoading = true;
      try {
        const res = await fetch(
          `${variables.basePath}/users/change-password/${$credentials.id}`,
          {
            method: "PATCH",
            body: JSON.stringify({
              id: $credentials.id,
              email: $credentials.email,
              password: oldPassword,
              newPassword,
            }),
            headers: {
              "content-type": "application/json",
            },
          }
        );
        const data = await res.json();
        if (res.status === 200) {
          notification.show("Contraseña modificada", "success");
          getModal().close();
        } else {
          let message = res.statusText
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

  onMount(() => {
    openModal = true;
  });

  function closeModal() {
    oldPassword = "";
    newPassword = "";
    confirmPassword = "";
  }
</script>

<button class="btn ripple " on:click={() => getModal().open(closeModal)}
  >Cambiar contraseña</button
>

{#if openModal}
  <Modal>
    <section class="container">
      <h1>Cambiar contraseña</h1>
      <form on:submit|preventDefault={formHandle}>
        <div>
          <div class="group">
            <input
              type="password"
              name="oldPassword"
              required="required"
              data-type="password"
              placeholder="Contraseña anterior"
              bind:value={oldPassword}
              on:input={resetFieldError}
            />
            {#if errors.oldPassword}
              <p class="error">
                <small style="color: red"> {errors.oldPassword} </small>
              </p>
            {/if}
          </div>
          <div class="group">
            <input
              type="password"
              name="newPassword"
              required="required"
              placeholder="Nueva contraseña"
              bind:value={newPassword}
              on:input={resetFieldError}
            />
            {#if errors.newPassword}
              <p class="error">
                <small style="color: red"> {errors.newPassword} </small>
              </p>
            {/if}
          </div>
          <div class="group">
            <input
              type="password"
              name="confirmPassword"
              required="required"
              placeholder="Confirmación contraseña"
              bind:value={confirmPassword}
              on:input={resetFieldError}
            />
            {#if errors.confirmPassword}
              <p class="error">
                <small style="color: red"> {errors.confirmPassword} </small>
              </p>
            {/if}
          </div>
          <button class="btn ripple btn-local">Modificar</button>
        </div>
      </form>

      {#if isLoading}
        <Spinner />
      {/if}
    </section>
  </Modal>
{/if}

<style>
  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .error {
    position: absolute;
    top: 13px;
    right: 15px;
  }
  input {
    margin-top: 25px;
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
  input::placeholder {
    letter-spacing: normal;
  }
  .btn-local {
    width: 100%;
    margin-top: 40px;
  }
  input[type="password"] {
    letter-spacing: 0.3em;
  }
  .group {
    position: relative;
  }
</style>
