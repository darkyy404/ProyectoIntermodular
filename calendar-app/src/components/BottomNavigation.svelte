
<script>
    let activeTab = "Calendario";
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
    <button
      class:active={activeTab === "Menú"}
      on:click={() => (activeTab = "Menú")}
    >
      ☰
    </button>
  </div>
  
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
  </style>