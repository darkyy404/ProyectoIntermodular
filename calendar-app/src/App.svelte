<script>
	import { navigate, Router, Route } from 'svelte-routing'; // Usar rutas
	import { onMount } from 'svelte';
	import Inicio from './components/Inicio.svelte'; // Componente de inicio
	import Calendario from './components/Calendar.svelte'; // Componente del calendario
	import MenuOptions from './components/MenuOptions.svelte'; // Componente de las opciones del menú
	import Contactos from './views/Contactos.svelte'; // Componente de contactos
	import RegistroPedidos from './components/RegistroPedidos.svelte'; // Componente de registro de pedidos
	import Configuraciones from './components/Configuraciones.svelte'; // Componente de configuraciones
	import ControlInventario from './components/ControlInventario.svelte'; // Componente de control de inventario
  
	let activeTab = "Calendario"; // Tab activo
	let menuOpen = false; // Estado del menú
	
	// Redirigir automáticamente a /Inicio cuando se carga el componente
	onMount(() => {
	  navigate('/Inicio');
	});
  
	// Función para abrir y cerrar el menú
	const toggleMenu = () => {
	  menuOpen = !menuOpen;
	};
  </script>
  
  <Router>
	<!-- Rutas para las diferentes páginas -->
	<Route path="/Inicio" component={Inicio} />
	<Route path="/calendario" component={Calendario} />
	<Route path="/menu-options" component={MenuOptions} /> <!-- Ruta a la pantalla de opciones -->
	<Route path="/contactos" component={Contactos} /> <!-- Ruta a los contactos -->
	<Route path="/registro-pedidos" component={RegistroPedidos} /> <!-- Ruta al registro de pedidos -->
	<Route path="/configuraciones" component={Configuraciones} /> <!-- Ruta a configuraciones -->
	<Route path="/control-inventario" component={ControlInventario} /> <!-- Ruta al control de inventario -->
  </Router>
  
  <!-- Barra de navegación en la parte inferior -->
  <div class="bottom-navigation">
	<button
	  class:active={activeTab === "Inicio"}
	  on:click={() => {
		activeTab = "Inicio";
		navigate('/Inicio'); // Redirigir a la página de inicio
	  }}
	>
	  🏠
	</button>
  
	<button
	  class:active={activeTab === "Calendario"}
	  on:click={() => {
		activeTab = "Calendario";
		navigate('/calendario'); // Redirigir a la página de calendario
	  }}
	>
	  📅
	</button>
  
	<!-- Icono de Contactos -->
	<button
	  class:active={activeTab === "Contactos"}
	  on:click={() => { 
		activeTab = "Contactos";
		navigate('/contactos') }}
	>
	  👥
	</button>
  
	<button
	  class:active={activeTab === "Menú"}
	  on:click={() => {
		activeTab = "Menú";
		navigate('/menu-options'); // Redirigir a la página de opciones
	  }}
	>
	  ☰
	</button>
  </div>
  
  <style>
	/* Estilos de la barra de navegación inferior */
	.bottom-navigation {
	  display: flex;
	  justify-content: space-around;
	  padding: 0.5rem 0;
	  background-color: #ffffff;
	  border-top: 1px solid #e0e0e0;
	  position: fixed;
	  bottom: 0;
	  width: 100%;
	  z-index: 10;
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
  </style>
  
>>>>>>> b222953 (Integrando mis archivos locales)
