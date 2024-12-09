<script>
  export let contacto; // Datos del contacto.
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher(); // Permite enviar eventos.

  const handleDelete = () => {
    dispatch('delete', contacto.id); // Evento para eliminar contacto.
  };

  const handleEdit = () => {
    dispatch('edit', contacto); // Evento para editar contacto.
  };
</script>

<style>
  .card {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
    position: relative;
    flex-wrap: wrap; /* Permite que los elementos se reorganicen en pantallas pequeñas */
  }

  .card:hover {
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }

  h3 {
    margin: 0;
    font-size: 1rem;
    color: #333;
    flex: 1 1 auto; /* Permite que el nombre ocupe más espacio en pantallas pequeñas */
  }

  p {
    margin: 0;
    font-size: 0.9rem;
    color: #a9b0c6;
    flex: 1 1 auto; /* Similar al título */
  }

  button {
    background: transparent;
    border: none;
    font-size: 1.5rem;
    color: #a9b0c6;
    cursor: pointer;
  }

  button:hover {
    color: #4a90e2;
  }

  .status-and-actions {
    display: flex;
    align-items: center;
    gap: 12px; /* Espacio entre el estado y los botones */
    flex: 0 0 auto; /* Fija el ancho para las acciones */
  }

  /* Media Queries */
  @media (max-width: 768px) {
    h3, p {
      font-size: 0.9rem;
    }

    button {
      font-size: 1.2rem;
    }

    .status-and-actions {
      gap: 8px; /* Reduce el espacio entre elementos en tablets */
    }
  }

  @media (max-width: 480px) {
    .card {
      flex-direction: column; /* Cambia el diseño a columnas */
      align-items: flex-start; /* Alinea el contenido al inicio */
      gap: 12px; /* Agrega espacio entre las secciones */
    }

    .status-and-actions {
      align-self: flex-end; /* Coloca los botones al final */
      gap: 6px; /* Reduce aún más el espacio entre elementos */
    }
  }
</style>

<div class="card">
  <div>
    <h3>{contacto.nombre}</h3>
    <p>{contacto.email}</p>
  </div>
  <div class="status-and-actions">
    <button on:click={handleEdit} aria-label="Editar contacto">✏️</button>
    <button on:click={handleDelete} aria-label="Eliminar contacto">🗑️</button>
  </div>
</div>
