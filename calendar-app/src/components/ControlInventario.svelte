<!-- ControlInventario.svelte -->
<script>
    // Aquí iría la lógica del inventario (por ejemplo, cargar productos, gestionarlos, etc.)
    let productos = [
      { id: 1, nombre: "Producto A", cantidad: 100 },
      { id: 2, nombre: "Producto B", cantidad: 50 },
      { id: 3, nombre: "Producto C", cantidad: 75 }
    ];
  </script>
  
  <h2>Control de Inventario</h2>
  
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Cantidad</th>
      </tr>
    </thead>
    <tbody>
      {#each productos as producto}
        <tr>
          <td>{producto.id}</td>
          <td>{producto.nombre}</td>
          <td>{producto.cantidad}</td>
        </tr>
      {/each}
    </tbody>
  </table>
  
  <style>
    h2 {
      text-align: center;
      margin-bottom: 20px;
    }
  
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
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
  </style>
  