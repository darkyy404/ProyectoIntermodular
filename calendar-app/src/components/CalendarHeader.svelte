<script>
    export let view; // Recibe la vista actual
    export let onViewChange; // Recibe la función de cambio de vista
  
    const handleViewChange = (newView) => {
      if (onViewChange) {
        onViewChange(newView); // Llama al evento para cambiar la vista
      }
    };
  </script>
  
  <div class="calendar-header">
    <button class="icon-button">🔍</button>
    <h1>Calendario</h1>
    <button class="icon-button">⚙️</button>
  
    <div class="view-selector">
      <button
        class:active={view === "Mes"}
        on:click={() => handleViewChange("Mes")}
      >
        Mes
      </button>
      <button
        class:active={view === "Día"}
        on:click={() => handleViewChange("Día")}
      >
        Día
      </button>
    </div>
  </div>
  
  <style>
    .calendar-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0.5rem 1rem;
      background-color: #ffffff;
      border-bottom: 1px solid #e0e0e0;
    }
  
    h1 {
      margin: 0;
      font-size: 1.25rem;
      font-weight: bold;
    }
  
    .icon-button {
      background: none;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
    }
  
    .view-selector {
      display: flex;
      gap: 0.5rem;
    }
  
    .view-selector button {
      padding: 0.5rem 1rem;
      border: none;
      background-color: #f0f0f0;
      cursor: pointer;
      border-radius: 4px;
      font-size: 0.9rem;
    }
  
    .view-selector button.active {
      background-color: #007bff;
      color: white;
    }
  </style>
  