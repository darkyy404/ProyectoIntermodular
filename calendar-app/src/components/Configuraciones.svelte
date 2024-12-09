<script>
    import { onMount } from 'svelte';
  
    // Variables de configuración
    let appName = 'Mi Aplicación';
    let theme = 'Claro'; // Opciones: Claro, Oscuro
    let notificationsEnabled = true;
  
    // Función para guardar la configuración
    const saveSettings = () => {
      // Guardar la configuración en localStorage
      localStorage.setItem('theme', theme);
      alert('Configuración guardada');
      
      // Aplicar el tema seleccionado
      if (theme === 'Oscuro') {
        document.body.classList.add('dark');
        document.body.classList.remove('light');
      } else {
        document.body.classList.add('light');
        document.body.classList.remove('dark');
      }
    };
  
    // Establecer el tema al inicio si está configurado en localStorage
    onMount(() => {
      const storedTheme = localStorage.getItem('theme');
      if (storedTheme) {
        theme = storedTheme; // Si hay un tema guardado, usamos ese
      }
  
      // Aplicamos el tema guardado (oscuro o claro) al cargar la página
      if (theme === 'Oscuro') {
        document.body.classList.add('dark');
        document.body.classList.remove('light');
      } else {
        document.body.classList.add('light');
        document.body.classList.remove('dark');
      }
    });
  </script>
  
  <h2>Configuraciones</h2>
  
  <div class="settings-form">
    <div class="setting-item">
      <label for="appName">Nombre de la Aplicación:</label>
      <input id="appName" type="text" bind:value={appName} />
    </div>
  
    <div class="setting-item">
      <label for="theme">Tema:</label>
      <select id="theme" bind:value={theme}>
        <option value="Claro">Claro</option>
        <option value="Oscuro">Oscuro</option>
      </select>
    </div>
  
    <div class="setting-item">
      <label for="notifications">Notificaciones:</label>
      <input id="notifications" type="checkbox" bind:checked={notificationsEnabled} />
      <span>{notificationsEnabled ? 'Activadas' : 'Desactivadas'}</span>
    </div>
  
    <button on:click={saveSettings}>Guardar Configuración</button>
  </div>
  
  <style>
    h2 {
      text-align: center;
      margin-top: 20px;
    }
  
    .settings-form {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 30px;
    }
  
    .setting-item {
      margin-bottom: 15px;
      width: 80%;
      max-width: 400px;
    }
  
    .setting-item label {
      display: block;
      margin-bottom: 5px;
      font-size: 1rem;
    }
  
    .setting-item input,
    .setting-item select {
      width: 100%;
      padding: 10px;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
  
    button {
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  
    /* Estilos para el tema claro */
    body.light {
      background-color: #ffffff;
      color: #000000;
      font-family: Arial, sans-serif;
    }
  
    /* Estilos para el tema oscuro */
    body.dark {
      background-color: #121212;
      color: #ffffff;
      font-family: Arial, sans-serif;
    }
  
    /* Estilos específicos para otros elementos si es necesario */
    .dark button {
      background-color: #444;
    }
  
    .dark button:hover {
      background-color: #666;
    }
  
    .light button {
      background-color: #007bff;
    }
  
    .light button:hover {
      background-color: #0056b3;
    }
  
    /* Opcional: Cambio de colores en inputs */
    body.light input, body.light select {
      background-color: #ffffff;
      color: #000;
    }
  
    body.dark input, body.dark select {
      background-color: #333;
      color: #fff;
    }
  </style>
  