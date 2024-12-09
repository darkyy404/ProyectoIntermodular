<script>
    export let producto;
    import { createEventDispatcher } from 'svelte';
  
    const dispatch = createEventDispatcher();
  
    const handleDelete = () => {
      dispatch('delete', producto.id);
    };
  
    const handleEdit = () => {
      dispatch('edit', producto);
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
    }
  
    h3 {
      margin: 0;
      font-size: 1rem;
      color: #333;
    }
  
    p {
      margin: 0;
      font-size: 0.9rem;
      color: #777;
    }
  
    .actions {
      display: flex;
      gap: 8px;
    }
  
    button {
      background: transparent;
      border: none;
      font-size: 1.2rem;
      cursor: pointer;
    }
  
    button:hover {
      color: #4a90e2;
    }
  </style>
  
  <div class="card">
    <div>
      <h3>{producto.nombre}</h3>
      <p>Precio: ${producto.precio}</p>
      <p>Stock: {producto.stock}</p>
    </div>
    <div class="actions">
      <button on:click={handleEdit}>✏️</button>
      <button on:click={handleDelete}>🗑️</button>
    </div>
  </div>
  