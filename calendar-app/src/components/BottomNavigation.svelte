<script>
  import { navigate } from 'svelte-routing'; // Importar la función navigate

  let activeTab = "Calendario"; // Tab activo
  let menuOpen = false; // Estado del menú

  // Función para abrir y cerrar el menú
  const toggleMenu = () => {
    menuOpen = !menuOpen;
  };
</script>

<div class="bottom-navigation">
  <!-- Botón de Inicio -->
  <button
    class:active={activeTab === "Inicio"}
    on:click={() => {
      activeTab = "Inicio";
      navigate('/Inicio'); // Redirigir a la página de inicio
    }}
  >
    🏠
  </button>
  
  <!-- Botón de Contactos -->
  <button
    class:active={activeTab === "Contactos"}
    on:click={() => (activeTab = "Contactos")}
  >
    👥
  </button>

  <!-- Botón de Calendario -->
  <button
    class:active={activeTab === "Calendario"}
    on:click={() => {
      activeTab = "Calendario";
      navigate('/'); // Redirigir a la página del calendario
    }}
  >
    📅
  </button>

  <!-- Menú hamburguesa (icono ☰) -->
  <button
    class:active={activeTab === "Menú"}
    on:click={() => {
      activeTab = "Menú";
      toggleMenu(); // Cambiar el estado del menú
    }}
  >
    ☰
  </button>
</div>

{#if menuOpen} <!-- Mostrar el menú solo cuando menuOpen sea true -->
  <div class="menu">
    <button on:click={() => { menuOpen = false; navigate("/registro-pedidos"); }}>Registro de pedidos</button>
    <button on:click={() => { menuOpen = false; }}>Opción 2</button>
    <button on:click={() => { menuOpen = false; }}>Opción 3</button>
    <button on:click={() => { menuOpen = false; }}>Opción 4</button>
  </div>
{/if}

<style>
  .bottom-navigation {
    display: flex;
    justify-content: space-around;
    padding: 0.5rem 0;
    background-color: #ffffff;
    border-top: 1px solid #e0e0e0;
    position: fixed;
    bottom: 0;
    width: 100%; /* Asegura que ocupe todo el ancho */
    z-index: 10; /* Para que esté por encima del contenido */
  }

  button {
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
  }

  button.active {
    color: #007bff;
  }

  /* Estilos del menú */
  .menu {
    position: absolute; /* Posición absoluta para que se sitúe respecto al contenedor */
    bottom: 60px; /* Ajusta este valor para que quede justo encima del botón */
    right: 10px; /* Ajusta la distancia desde el borde derecho */
    background-color: white;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 200px;
    z-index: 20; /* Asegura que el menú esté por encima de otros elementos */
  }

  .menu button {
    margin: 10px;
    padding: 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .menu button:hover {
    background-color: #45a049;
  }
</style>
