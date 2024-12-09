<script>
  import { onMount } from 'svelte';

  export let isOpen = false; // Estado para mostrar/ocultar modal.
  export let title = ''; // Título dinámico del modal.
  export let contact = { nombre: '', email: '' }; // Datos del contacto.
  export let onSubmit; // Función para manejar la acción de guardar.
  let modalElement;

  // Maneja el envío del formulario del modal.
  const handleSubmit = () => {
    onSubmit(contact);
    isOpen = false;
  };

  // Cierra el modal cuando se hace clic fuera de él.
  const closeModal = (event) => {
    if (event.target.classList.contains('overlay')) {
      isOpen = false;
    }
  };

  onMount(() => {
    if (isOpen && modalElement) {
      modalElement.focus();
    }
  });
</script>

<style>
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5); /* Fondo semitransparente */
    display: flex; /* Centra el modal */
    justify-content: center;
    align-items: center;
    z-index: 999;
  }

  .modal {
    background: white;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2); /* Sombra */
    width: 90%; /* Ancho relativo */
    max-width: 400px; /* Máximo ancho en escritorio */
    animation: fadeIn 0.3s ease-in-out;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  h2 {
    margin: 0 0 10px 0;
    font-size: 1.5rem;
    color: #333;
    text-align: center; /* Centra el título */
  }

  label {
    display: block;
    font-size: 0.9rem;
    color: #555;
    margin-top: 10px;
  }

  input {
    width: 100%;
    padding: 10px;
    font-size: 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    outline: none;
    margin-top: 5px;
  }

  input:focus {
    border-color: #4a90e2;
  }

  .buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
    gap: 12px;
  }

  button {
    flex: 1;
    padding: 10px;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    cursor: pointer;
  }

  button:first-child {
    background: #4a90e2;
    color: white;
  }

  button:last-child {
    background: #f3f5f8;
    color: #555;
  }

  button:hover {
    opacity: 0.9;
  }

  /* Media Queries */
  @media (max-width: 768px) {
    .modal {
      width: 95%;
      padding: 16px;
    }

    h2 {
      font-size: 1.3rem;
    }

    input {
      font-size: 0.9rem;
    }

    button {
      font-size: 0.9rem;
      padding: 8px;
    }
  }

  @media (max-width: 480px) {
    .buttons {
      flex-direction: column; /* Botones en columnas */
    }

    button {
      margin-top: 8px; /* Espacio entre botones */
    }

    h2 {
      font-size: 1.2rem;
    }
  }
</style>

{#if isOpen}
<div
  class="overlay"
  on:click={closeModal}
  on:keydown={(e) => e.key === 'Escape' && (isOpen = false)}>
  <div class="modal" bind:this={modalElement}>
    <h2>{title}</h2>
    <label for="nombre">
      Nombre:
      <input
        id="nombre"
        type="text"
        bind:value={contact.nombre}
        placeholder="Introduce el nombre"
        aria-label="Nombre del contacto"
      />
    </label>
    <label for="email">
      Email:
      <input
        id="email"
        type="email"
        bind:value={contact.email}
        placeholder="Introduce el email"
        aria-label="Correo electrónico del contacto"
      />
    </label>
    <div class="buttons">
      <button on:click={handleSubmit} aria-label="Guardar los cambios">Guardar</button>
      <button on:click={() => (isOpen = false)} aria-label="Cancelar los cambios">Cancelar</button>
    </div>
  </div>
</div>
{/if}
