<script>
	import CalendarHeader from "./components/CalendarHeader.svelte";
	import CalendarView from "./components/CalendarView.svelte";
	import BottomNavigation from "./components/BottomNavigation.svelte";
  
	let view = "Mes"; // Vista inicial (Mes o Día)
	let selectedDate = new Date(); // Fecha seleccionada (inicia con hoy)
  
	const changeView = (newView) => {
	  view = newView; // Cambia entre "Mes" y "Día"
	};
  
	const updateSelectedDate = (date) => {
	  selectedDate = date; // Actualiza la fecha seleccionada
	};
  </script>
  
  <main>
	<!-- Encabezado -->
	<CalendarHeader {view} onViewChange={changeView} />
	
	<!-- Vista principal del calendario -->
	<CalendarView {view} {selectedDate} onDateChange={updateSelectedDate} />
	
	<!-- Barra de navegación inferior -->
	<BottomNavigation />
  
  </main>
  
  <style>
	main {
	  font-family: Arial, sans-serif;
	  display: flex;
	  flex-direction: column;
	  height: 100vh;
	  justify-content: space-between;
	  background-color: #f9f9f9;
	}
  </style>
  