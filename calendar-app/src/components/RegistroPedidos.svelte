<script>
    // Cargar los pedidos desde localStorage al iniciar
    let pedidos = JSON.parse(localStorage.getItem('pedidos')) || [];
  
    let newPedido = { cliente: '', fecha: '', total: '' }; // Datos del nuevo pedido
    let showForm = false; // Mostrar formulario para agregar un nuevo pedido
  
    // Función para agregar un nuevo pedido
    const addPedido = () => {
      if (newPedido.cliente && newPedido.fecha && newPedido.total) {
        // Obtener el id basado en el último pedido o 1 si no hay pedidos
        const nuevoId = pedidos.length > 0 ? pedidos[pedidos.length - 1].id + 1 : 1;
        
        const nuevoPedido = { ...newPedido, id: nuevoId };
        pedidos = [...pedidos, nuevoPedido]; // Actualizamos el array de forma reactiva
  
        // Guardamos los pedidos en localStorage
        localStorage.setItem('pedidos', JSON.stringify(pedidos));
  
        showForm = false; // Cerrar formulario después de agregar
        newPedido = { cliente: '', fecha: '', total: '' }; // Resetear el formulario
      } else {
        alert('Por favor complete todos los campos');
      }
    };
  
    // Función para eliminar un pedido
    const deletePedido = (id) => {
      pedidos = pedidos.filter(pedido => pedido.id !== id); // Elimina el pedido por ID
  
      // Guardamos los pedidos actualizados en localStorage
      localStorage.setItem('pedidos', JSON.stringify(pedidos));
    };
  </script>
  
  <h2>Registro de Pedidos</h2>
  
  <!-- Botón para mostrar el formulario de nuevo pedido -->
  <button class="add-button" on:click={() => showForm = true}>Agregar Pedido</button>
  
  <!-- Formulario para agregar un nuevo pedido -->
  {#if showForm}
    <div class="form-container">
      <input type="text" placeholder="Cliente" bind:value={newPedido.cliente} />
      <input type="date" bind:value={newPedido.fecha} />
      <input type="text" placeholder="Total" bind:value={newPedido.total} />
  
      <div class="form-actions">
        <button class="save-button" on:click={addPedido}>Guardar Pedido</button>
        <button class="cancel-button" on:click={() => showForm = false}>Cancelar</button>
      </div>
    </div>
  {/if}
  
  <!-- Tabla de pedidos existentes -->
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Cliente</th>
          <th>Fecha</th>
          <th>Total</th>
          <th>Acciones</th> <!-- Nueva columna para el botón de eliminar -->
        </tr>
      </thead>
      <tbody>
        {#each pedidos as pedido}
          <tr>
            <td>{pedido.id}</td>
            <td>{pedido.cliente}</td>
            <td>{pedido.fecha}</td>
            <td>{pedido.total}</td>
            <td>
              <!-- Botón de eliminar -->
              <button class="delete-button" on:click={() => deletePedido(pedido.id)}>Eliminar</button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  
  <style>
    h2 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 1.8rem;
      color: #333;
    }
  
    /* Botón de agregar */
    .add-button {
      padding: 10px 20px;
      background-color: #28a745;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 1rem;
      cursor: pointer;
      margin: 20px auto;
      display: block;
      transition: background-color 0.3s ease;
    }
  
    .add-button:hover {
      background-color: #218838;
    }
  
    /* Contenedor del formulario */
    .form-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #f9f9f9;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      margin-bottom: 20px;
    }
  
    .form-container input {
      width: 250px;
      padding: 10px;
      margin-bottom: 10px;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 5px;
      transition: border-color 0.3s;
    }
  
    .form-container input:focus {
      border-color: #007bff;
      outline: none;
    }
  
    .form-actions {
      display: flex;
      justify-content: space-between;
      width: 100%;
    }
  
    .save-button, .cancel-button {
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      font-size: 1rem;
      cursor: pointer;
    }
  
    .save-button {
      background-color: #007bff;
      color: white;
    }
  
    .save-button:hover {
      background-color: #0056b3;
    }
  
    .cancel-button {
      background-color: #dc3545;
      color: white;
    }
  
    .cancel-button:hover {
      background-color: #c82333;
    }
  
    /* Estilos para la tabla */
    .table-container {
      width: 80%;
      margin: 0 auto;
    }
  
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
  
    th, td {
      padding: 12px;
      text-align: center;
      border: 1px solid #ddd;
    }
  
    th {
      background-color: #007bff;
      color: white;
      font-weight: bold;
    }
  
    tr:nth-child(even) {
      background-color: #f2f2f2;
    }
  
    tr:hover {
      background-color: #f1f1f1;
    }
  
    /* Estilos para el botón de eliminar */
    .delete-button {
      padding: 6px 12px;
      background-color: #dc3545;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  
    .delete-button:hover {
      background-color: #c82333;
    }
  
    /* Estilos responsivos */
    @media (max-width: 600px) {
      .form-container input {
        width: 100%;
      }
  
      .form-actions {
        flex-direction: column;
        align-items: center;
      }
  
      .save-button, .cancel-button {
        width: 100%;
        margin-top: 10px;
      }
  
      .delete-button {
        width: 100%;
        margin-top: 10px;
      }
    }
  </style>
  