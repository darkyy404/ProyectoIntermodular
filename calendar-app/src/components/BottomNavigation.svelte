<script>
    let activeTab = "Calendario"; // Tab activo
    let menuOpen = false; // Estado del menú
  
    // Función para abrir y cerrar el menú
    const toggleMenu = () => {
      menuOpen = !menuOpen;
    };
  </script>
  
  <div class="bottom-navigation">
    <button
      class:active={activeTab === "Inicio"}
      on:click={() => (activeTab = "Inicio")}
    >
      🏠
    </button>
    <button
      class:active={activeTab === "Contactos"}
      on:click={() => (activeTab = "Contactos")}
    >
      👥
    </button>
    <button
      class:active={activeTab === "Calendario"}
      on:click={() => (activeTab = "Calendario")}
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
      <button on:click={() => { menuOpen = false; }}>Opción 1</button>
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
      position: fixed;
      bottom: 80px;
      left: 20px;
      background-color: white;
      border: 1px solid #ccc;
      border-radius: 5px;
      padding: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      width: 200px;
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
  