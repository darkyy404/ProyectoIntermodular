<script>
  import BottomNavigation from './BottomNavigation.svelte'; // Importar el componente de navegación
  import CalendarView from './CalendarView.svelte'; // El calendario
</script>

<div class="home">
  <!-- Aquí puedes agregar otros componentes o contenido de la página principal -->
  
  <h1>Bienvenido al calendario</h1>
  
  <!-- Mostrar el calendario -->
  <CalendarView view="Mes" selectedDate={new Date()} />

  <!-- Incluir la barra de navegación en la parte inferior -->
  <BottomNavigation />
</div>

<style>
  .home {
    display: flex;
    flex-direction: column;
    height: 100vh;
    padding-bottom: 60px; /* Para evitar que se solape con la barra de navegación */
  }
</style>
