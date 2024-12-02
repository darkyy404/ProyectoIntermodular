<script>
  // Cargar los productos desde localStorage al iniciar
  let productos = JSON.parse(localStorage.getItem('productos')) || [
    { id: 1, nombre: "Producto A", cantidad: 100 },
    { id: 2, nombre: "Producto B", cantidad: 50 },
    { id: 3, nombre: "Producto C", cantidad: 75 }
  ];

  // Datos para un nuevo producto o el producto que estamos editando
  let newProducto = { nombre: '', cantidad: 0 }; 
  let editingProducto = null; // Producto que estamos editando
  let showForm = false; // Controlar la visibilidad del formulario

  // Variable que usaremos para el binding de los valores
  let productoActual = { nombre: '', cantidad: 0 }; // Valor para el formulario (nuevo o editado)

  // Función para agregar un nuevo producto
  const agregarProducto = () => {
    if (productoActual.nombre && productoActual.cantidad > 0) {
      const nuevoId = productos.length > 0 ? productos[productos.length - 1].id + 1 : 1;
      const nuevoProducto = { ...productoActual, id: nuevoId };
      productos = [...productos, nuevoProducto]; // Actualizamos de forma reactiva
      localStorage.setItem('productos', JSON.stringify(productos)); // Guardar productos en localStorage
      productoActual = { nombre: '', cantidad: 0 }; // Resetear los campos del formulario
      showForm = false; // Cerrar formulario
    } else {
      alert('Por favor complete todos los campos correctamente');
    }
  };

  // Función para eliminar un producto
  const eliminarProducto = (id) => {
    productos = productos.filter(producto => producto.id !== id); // Filtrar el producto que se elimina
    localStorage.setItem('productos', JSON.stringify(productos)); // Guardar productos actualizados
  };

  // Función para editar un producto
  const editarProducto = (producto) => {
    editingProducto = { ...producto }; // Cargar el producto en los campos del formulario
    productoActual = { ...producto }; // Cargar en productoActual los datos del producto editado
    showForm = true; // Mostrar el formulario para editar
  };

  // Función para actualizar un producto
  const actualizarProducto = () => {
    if (productoActual.nombre && productoActual.cantidad > 0) {
      productos = productos.map(producto =>
        producto.id === productoActual.id ? productoActual : producto
      ); // Actualizar el producto en el array
      localStorage.setItem('productos', JSON.stringify(productos)); // Guardar productos actualizados
      editingProducto = null; // Resetear el producto en edición
      showForm = false; // Cerrar el formulario
    } else {
      alert('Por favor complete todos los campos correctamente');
    }
  };

  // Mostrar el formulario cuando el usuario hace clic en "Agregar Producto"
  const mostrarFormulario = () => {
    showForm = true;
    editingProducto = null; // Asegurar que no estamos editando ningún producto
    productoActual = { nombre: '', cantidad: 0 }; // Resetear los campos
  };
</script>

<h2>Control de Inventario</h2>

<!-- Barra de acción de inventario -->
<div class="actions">
  <button class="add-button" on:click={mostrarFormulario}>Agregar Producto</button>
</div>

<!-- Formulario de agregar/editar producto -->
{#if showForm}
  <div class="form-container">
    <!-- Para agregar/editar el nombre y cantidad del producto -->
    <input type="text" placeholder="Nombre del Producto" bind:value={productoActual.nombre} />
    <input type="number" placeholder="Cantidad" bind:value={productoActual.cantidad} />
    <div class="form-actions">
      {#if editingProducto}
        <button class="save-button" on:click={actualizarProducto}>Actualizar Producto</button>
      {:else}
        <button class="save-button" on:click={agregarProducto}>Guardar Producto</button>
      {/if}
      <button class="cancel-button" on:click={() => showForm = false}>Cancelar</button>
    </div>
  </div>
{/if}

<!-- Tabla de productos -->
<div class="table-container">
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Cantidad</th>
        <th>Acciones</th> <!-- Columna de acciones (editar/eliminar) -->
      </tr>
    </thead>
    <tbody>
      {#each productos as producto}
        <tr>
          <td>{producto.id}</td>
          <td>{producto.nombre}</td>
          <td>{producto.cantidad}</td>
          <td>
            <button class="edit-btn" on:click={() => editarProducto(producto)}>Editar</button>
            <button class="delete-btn" on:click={() => eliminarProducto(producto.id)}>Eliminar</button>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>

<style>
  h2 {
    text-align: center;
    margin-bottom: 30px;
    font-size: 2rem;
    color: #333;
  }

  /* Barra de acciones */
  .actions {
    display: flex;
    justify-content: flex-start;
    gap: 20px;
    margin-bottom: 20px;
    padding: 10px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }

  .actions button {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .actions .add-button {
    background-color: #28a745;
    color: white;
  }

  .actions .add-button:hover {
    background-color: #218838;
  }

  /* Estilos para el formulario */
  .form-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #f9f9f9;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
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
    margin-top: 20px;
    overflow-x: auto;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    background-color: #fff;
  }

  th, td {
    padding: 15px;
    text-align: center;
    border: 1px solid #ddd;
    font-size: 1rem;
  }

  th {
    background-color: #007bff;
    color: white;
    font-weight: bold;
  }

  td {
    background-color: #f9f9f9;
  }

  tr:nth-child(even) td {
    background-color: #f2f2f2;
  }

  tr:hover {
    background-color: #f1f1f1;
  }

  .edit-btn, .delete-btn {
    padding: 6px 12px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .edit-btn {
    background-color: #ffc107;
    color: white;
  }

  .edit-btn:hover {
    background-color: #e0a800;
  }

  .delete-btn {
    background-color: #dc3545;
    color: white;
  }

  .delete-btn:hover {
    background-color: #c82333;
  }

  /* Estilos responsivos */
  @media (max-width: 600px) {
    .actions {
      flex-direction: column;
      align-items: center;
    }

    .actions button {
      width: 100%;
      margin-bottom: 10px;
    }

    table {
      font-size: 0.9rem;
    }

    th, td {
      padding: 10px;
    }
  }
</style>
