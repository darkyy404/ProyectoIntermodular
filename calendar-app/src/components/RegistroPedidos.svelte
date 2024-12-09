<script>
  // Cargar los productos y pedidos desde localStorage al iniciar
  let productos = JSON.parse(localStorage.getItem('productos')) || [
    { id: 1, nombre: "Producto A", cantidad: 100, precio: 10 },
    { id: 2, nombre: "Producto B", cantidad: 50, precio: 20 },
    { id: 3, nombre: "Producto C", cantidad: 75, precio: 15 }
  ];

  let pedidos = JSON.parse(localStorage.getItem('pedidos')) || [];

  let newPedido = { cliente: '', fecha: '', total: 0, productosSeleccionados: [] }; // Datos del nuevo pedido
  let showForm = false; // Mostrar formulario para agregar un nuevo pedido

  // Función para agregar un nuevo pedido
  const addPedido = () => {
    if (newPedido.cliente && newPedido.fecha && newPedido.total > 0 && newPedido.productosSeleccionados.length > 0) {
      // Obtener el id basado en el último pedido o 1 si no hay pedidos
      const nuevoId = pedidos.length > 0 ? pedidos[pedidos.length - 1].id + 1 : 1;

      const nuevoPedido = { ...newPedido, id: nuevoId };
      pedidos = [...pedidos, nuevoPedido]; // Actualizamos el array de forma reactiva

      // Guardamos los pedidos en localStorage
      localStorage.setItem('pedidos', JSON.stringify(pedidos));

      // Actualizamos el inventario en localStorage
      localStorage.setItem('productos', JSON.stringify(productos));

      // Resetear formulario
      showForm = false; 
      newPedido = { cliente: '', fecha: '', total: 0, productosSeleccionados: [] };
    } else {
      alert('Por favor complete todos los campos');
    }
  };

  // Función para eliminar un pedido y restaurar los productos
  const deletePedido = (id) => {
    const pedidoEliminado = pedidos.find(pedido => pedido.id === id); // Encontramos el pedido eliminado

    // Restaurar los productos del pedido eliminado al inventario
    pedidoEliminado.productosSeleccionados.forEach(producto => {
      const productoEnInventario = productos.find(p => p.id === producto.id);
      if (productoEnInventario) {
        productoEnInventario.cantidad += 1; // Sumamos la cantidad al inventario
      }
    });

    // Actualizamos el inventario en localStorage
    localStorage.setItem('productos', JSON.stringify(productos));

    // Eliminamos el pedido de la lista
    pedidos = pedidos.filter(pedido => pedido.id !== id);

    // Guardamos los pedidos actualizados en localStorage
    localStorage.setItem('pedidos', JSON.stringify(pedidos));
  };

  // Función para seleccionar/deseleccionar productos en el pedido
  const toggleProductoSeleccionado = (producto) => {
    const index = newPedido.productosSeleccionados.findIndex(p => p.id === producto.id);
    if (index === -1) {
      if (producto.cantidad > 0) { // Solo permitir seleccionar si hay stock disponible
        newPedido.productosSeleccionados.push(producto);
        producto.cantidad--; // Restamos uno del inventario
        localStorage.setItem('productos', JSON.stringify(productos)); // Guardar cambios en el inventario
      } else {
        alert('No hay suficiente stock disponible para este producto.');
      }
    } else {
      newPedido.productosSeleccionados.splice(index, 1); // Deseleccionamos el producto
      producto.cantidad++; // Revertimos el cambio en el inventario
      localStorage.setItem('productos', JSON.stringify(productos)); // Guardar cambios en el inventario
    }

    // Calcular el total del pedido basado en los productos seleccionados
    newPedido.total = newPedido.productosSeleccionados.reduce((acc, p) => acc + (p.precio * 1), 0);
  };

  // Función para permitir la edición manual del total
  const updateTotal = (event) => {
    newPedido.total = parseFloat(event.target.value);
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

    <div class="productos-container">
      <h3>Selecciona productos</h3>
      <ul>
        {#each productos as producto}
          <li>
            <label>
              <input type="checkbox" on:change={() => toggleProductoSeleccionado(producto)} disabled={producto.cantidad === 0} />
              {producto.nombre} - ${producto.precio} - Stock: {producto.cantidad}
            </label>
          </li>
        {/each}
      </ul>
    </div>

    <!-- Campo para ingresar el total manualmente -->
    <input type="number" placeholder="Total del Pedido" bind:value={newPedido.total} on:input={updateTotal} />

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
          <td>${pedido.total}</td>
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

  .productos-container {
    margin-bottom: 20px;
  }

  .productos-container ul {
    list-style: none;
    padding: 0;
  }

  .productos-container li {
    margin-bottom: 10px;
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
