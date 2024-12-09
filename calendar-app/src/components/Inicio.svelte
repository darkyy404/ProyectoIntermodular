<script>
    // Datos de ejemplo para ganancias, pedidos y clientes
    let stats = {
      totalClients: 120,
      totalOrders: 250,
      totalRevenue: 50000, // en USD
      totalProfit: 15000, // en USD
    };
  
    let recentOrders = [
      { orderId: '#1245', client: 'Juan Pérez', amount: 300, date: '2024-11-25' },
      { orderId: '#1246', client: 'Ana Gómez', amount: 150, date: '2024-11-24' },
      { orderId: '#1247', client: 'Carlos Rodríguez', amount: 450, date: '2024-11-23' }
    ];
  
    let clientList = [
      { name: 'Juan Pérez', email: 'juan@example.com', phone: '123-456-7890' },
      { name: 'Ana Gómez', email: 'ana@example.com', phone: '234-567-8901' },
      { name: 'Carlos Rodríguez', email: 'carlos@example.com', phone: '345-678-9012' }
    ];
  </script>
  
  <h1>Panel de Información</h1>
  
  <!-- Resumen de estadísticas -->
  <div class="stats-summary">
    <h2>Estadísticas Generales</h2>
    <p><strong>Total de Clientes:</strong> {stats.totalClients}</p>
    <p><strong>Total de Pedidos:</strong> {stats.totalOrders}</p>
    <p><strong>Ganancias Totales:</strong> ${stats.totalRevenue}</p>
    <p><strong>Beneficios Totales:</strong> ${stats.totalProfit}</p>
  </div>
  
  <!-- Tabla de pedidos recientes -->
  <div class="recent-orders">
    <h3>Pedidos Recientes</h3>
    <table>
      <thead>
        <tr>
          <th>ID Pedido</th>
          <th>Cliente</th>
          <th>Monto</th>
          <th>Fecha</th>
        </tr>
      </thead>
      <tbody>
        {#each recentOrders as order}
          <tr>
            <td>{order.orderId}</td>
            <td>{order.client}</td>
            <td>${order.amount}</td>
            <td>{order.date}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  
  <!-- Tabla de clientes -->
  <div class="client-list">
    <h3>Lista de Clientes</h3>
    <table>
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Email</th>
          <th>Teléfono</th>
        </tr>
      </thead>
      <tbody>
        {#each clientList as client}
          <tr>
            <td>{client.name}</td>
            <td>{client.email}</td>
            <td>{client.phone}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  
  <style>
    h1, h2, h3 {
      color: #333;
    }
  
    .stats-summary {
      background-color: #f9f9f9;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 20px;
    }
  
    .stats-summary p {
      font-size: 1.2rem;
    }
  
    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }
  
    th, td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }
  
    th {
      background-color: #f2f2f2;
    }
  
    tr:hover {
      background-color: #f1f1f1;
    }
  </style>
  