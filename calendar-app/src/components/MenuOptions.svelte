<script>
    import { navigate } from 'svelte-routing'; // Importamos navigate
  
    // Función para redirigir a la opción seleccionada
    const selectOption = (option) => {
      switch (option) {
        case 'registro':
          navigate('/registro-pedidos'); // Usamos navigate para cambiar la ruta
          break;
        case 'config':
          navigate('/configuraciones'); // Redirige a Configuraciones
          break;
        case 'reportes':
          navigate('/reportes'); // Redirige a Reportes
          break;
        case 'contactos':
          navigate('/contactos'); // Redirige a Contactos
          break;
        default:
          break;
      }
    };
  </script>
  
  <div class="menu-container">
    <h2>Selecciona una opción</h2>
  
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="menu-option" on:click={() => selectOption('registro')}>
      <h3>Registro de Pedidos</h3>
      <p>Gestiona los pedidos realizados en el sistema.</p>
    </div>
  
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="menu-option" on:click={() => selectOption('config')}>
      <h3>Configuraciones</h3>
      <p>Configura los parámetros del sistema.</p>
    </div>
  </div>
  
  <style>
    .menu-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 50px;
    }
  
    .menu-container h2 {
      margin-bottom: 30px;
    }
  
    .menu-option {
      background-color: #f4f4f4;
      border: 1px solid #ddd;
      padding: 20px;
      width: 80%;
      margin-bottom: 15px;
      cursor: pointer;
      border-radius: 8px;
      text-align: center;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
  
    .menu-option:hover {
      transform: scale(1.05);
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
    }
  
    .menu-option h3 {
      font-size: 1.2rem;
      margin-bottom: 10px;
    }
  
    .menu-option p {
      font-size: 1rem;
      color: #555;
    }
  </style>
  