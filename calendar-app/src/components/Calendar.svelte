<script>
    import { onMount, onDestroy } from 'svelte'; // Importar onDestroy también
    import { Calendar } from '@fullcalendar/core';
    import dayGridPlugin from '@fullcalendar/daygrid';
    import interactionPlugin from '@fullcalendar/interaction'; // Habilitar interactividad
    import '@fullcalendar/core/main.css'; // Importar estilos
    import '@fullcalendar/daygrid/main.css'; // Importar estilos de la vista de día
  
    let calendar;
    let showModal = false; // Controla la visibilidad de la ventana modal
    let eventTitle = ''; // Título del evento
    let eventDescription = ''; // Descripción del evento
    let eventDate = ''; // Fecha del evento
    let eventToEdit = null; // Evento que se está editando
    let events = []; // Lista de eventos
  
    // Cargar eventos desde localStorage al cargar la página
    const loadEvents = () => {
      const storedEvents = localStorage.getItem('events');
      if (storedEvents) {
        events = JSON.parse(storedEvents); // Parsear los eventos guardados
      }
    };
  
    // Guardar eventos en localStorage
    const saveEvents = () => {
      localStorage.setItem('events', JSON.stringify(events)); // Guardar los eventos en localStorage
    };
  
    // Función para agregar eventos
    const addEvent = (info) => {
      eventDate = info.dateStr; // La fecha seleccionada
      eventTitle = ''; // Reseteamos el título
      eventDescription = ''; // Reseteamos la descripción
      eventToEdit = null; // No estamos editando ningún evento
  
      showModal = true; // Mostramos la ventana modal
    };
  
    // Función para editar eventos
    const editEvent = (event) => {
      eventTitle = event.title;
      eventDescription = event.extendedProps.description || '';
      eventDate = event.startStr; // Fecha del evento a editar
      eventToEdit = event; // Guardamos el evento que estamos editando
  
      showModal = true; // Mostramos la ventana modal
    };
  
    // Función para guardar el evento (nuevo o editado)
    const saveEvent = () => {
      if (eventToEdit) {
        // Si estamos editando un evento, actualizamos su título y descripción
        eventToEdit.setProp('title', eventTitle);
        eventToEdit.setExtendedProp('description', eventDescription);
      } else {
        // Si es un evento nuevo, lo agregamos
        const newEvent = {
          title: eventTitle,
          start: eventDate,
          description: eventDescription,
          allDay: true
        };
        events.push(newEvent); // Agregamos el nuevo evento a la lista
        calendar.addEvent(newEvent); // Añadimos el evento al calendario
      }
  
      saveEvents(); // Guardamos los eventos en localStorage
      showModal = false; // Ocultamos la ventana modal después de guardar
    };
  
    // Función para cancelar y cerrar la ventana modal
    const cancelEvent = () => {
      showModal = false; // Cerramos la ventana modal sin guardar
    };
  
    // Función para eliminar un evento
    const deleteEvent = () => {
      if (eventToEdit) {
        // Eliminamos el evento de la interfaz del calendario
        eventToEdit.remove();
  
        // Eliminamos el evento de la lista de eventos
        events = events.filter(event => eventToEdit.startStr !== event.start);
  
        saveEvents(); // Guardamos los eventos actualizados en localStorage
        showModal = false; // Cerramos la ventana modal
      }
    };
  
    onMount(() => {
      loadEvents(); // Cargar eventos desde localStorage al cargar la página
  
      // Crear la instancia del calendario
      calendar = new Calendar(document.getElementById('calendar'), {
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: 'dayGridMonth', // Vista inicial
        events: events, // Cargar eventos desde la lista
        editable: true, // Permitir edición de eventos
        droppable: true, // Permitir arrastrar y soltar
        dateClick: addEvent, // Agregar evento al hacer clic en una fecha
        eventClick: (info) => editEvent(info.event), // Editar evento al hacer clic en él
      });
  
      // Renderizar el calendario
      calendar.render();
    });
  
    // Destruir el calendario al desmontar el componente
    onDestroy(() => {
      if (calendar) {
        calendar.destroy();
      }
    });
  </script>
  
  <div id="calendar"></div>
  
  <!-- Ventana Modal para agregar/editar evento -->
  {#if showModal}
    <div class="modal">
      <div class="modal-content">
        <h3>{eventToEdit ? 'Editar Evento' : 'Agregar Evento'}</h3>
        <label for="title">Título:</label>
        <input id="title" type="text" bind:value={eventTitle} placeholder="Ingrese el título del evento" />
  
        <label for="description">Descripción:</label>
        <textarea id="description" bind:value={eventDescription} placeholder="Ingrese una descripción"></textarea>
  
        <div class="modal-actions">
          <button on:click={saveEvent}>Guardar</button>
          {#if eventToEdit} <!-- Mostrar el botón de eliminar solo cuando estamos editando -->
            <button on:click={deleteEvent} style="background-color: red;">Eliminar</button>
          {/if}
          <button on:click={cancelEvent}>Cancelar</button>
        </div>
      </div>
    </div>
  {/if}
  
  <style>
    #calendar {
      width: 100%;
      height: 600px;
      margin: 0 auto;
    }
  
    /* Estilo de la ventana modal */
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 100;
    }
  
    .modal-content {
      background-color: white;
      padding: 20px;
      border-radius: 8px;
      width: 400px;
    }
  
    .modal h3 {
      margin-top: 0;
    }
  
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }
  
    input, textarea {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
  
    .modal-actions {
      display: flex;
      justify-content: space-between;
    }
  
    .modal-actions button {
      padding: 10px 20px;
      border: none;
      background-color: #007bff;
      color: white;
      cursor: pointer;
      border-radius: 5px;
    }
  
    .modal-actions button:hover {
      background-color: #0056b3;
    }
  
    .modal-actions button[style*="background-color: red"] {
      background-color: red;
    }
  </style>
  