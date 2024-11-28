<script>
	import { Router, Route } from 'svelte-routing';
	import Home from './components/Home.svelte'; // Componente principal con el menú
	import RegistroPedidos from './pages/RegistroPedidos.svelte'; // Componente de registro de pedidos
	import CalendarView from './components/CalendarView.svelte'; // El componente del calendario
  </script>
  
  <main>
	<Router>
	  <!-- Ruta para el componente Home (con el menú y el calendario) -->
	  <Route path="/" component={Home} />
	  <!-- Ruta para la página de registro de pedidos -->
	  <Route path="/registro-pedidos" component={RegistroPedidos} />
	  <!-- Ruta para la vista del calendario -->
	  <Route path="/calendario" component={CalendarView} />
	</Router>
  </main>
  