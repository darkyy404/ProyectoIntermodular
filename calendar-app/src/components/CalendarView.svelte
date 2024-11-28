<script>
	import { onMount, tick } from "svelte";
	import { Calendar } from "@fullcalendar/core";
	import dayGridPlugin from "@fullcalendar/daygrid";
	import interactionPlugin from "@fullcalendar/interaction";
	import "@fullcalendar/daygrid/main.css";
	import { format, addDays, subDays } from "date-fns";
  
	export let view; // Vista actual ("Mes" o "Día")
	export let selectedDate; // Fecha seleccionada
	export let onDateChange; // Función para actualizar la fecha seleccionada
  
	let calendarEl; // Referencia al contenedor del calendario
	let calendar; // Instancia de FullCalendar
	let events = []; // Eventos cargados dinámicamente
  
	let showModal = false; // Controla la visibilidad del modal para añadir eventos
	let showEventModal = false; // Controla la visibilidad del modal para ver/editar un evento
	let selectedEvent = null; // Evento actualmente seleccionado
  
	let newEventTitle = ""; // Título del nuevo evento
	let newEventDescription = ""; // Descripción del nuevo evento
  
	// Cargar eventos desde localStorage al iniciar
	onMount(() => {
	  const savedEvents = localStorage.getItem("events");
	  if (savedEvents) {
		events = JSON.parse(savedEvents); // Cargar eventos guardados
	  }
  
	  if (view === "Mes") {
		initCalendar();
	  }
	});
  
	// Guardar eventos en localStorage
	const saveEventsToLocalStorage = () => {
	  localStorage.setItem("events", JSON.stringify(events));
	};
  
	// Inicializar FullCalendar
	const initCalendar = async () => {
	  await tick(); // Asegurarnos de que el DOM esté actualizado
	  if (calendarEl) {
		// Destruir cualquier instancia previa
		destroyCalendar();
  
		// Crear una nueva instancia de FullCalendar
		calendar = new Calendar(calendarEl, {
		  plugins: [dayGridPlugin, interactionPlugin],
		  initialView: "dayGridMonth",
		  events: events,
		  dateClick: function (info) {
			// Abrir modal al hacer clic en un día
			selectedDate = new Date(info.dateStr);
			showModal = true;
		  },
		  eventClick: function (info) {
			// Abrir modal al hacer clic en un evento
			const clickedEvent = events.find(
			  (event) => event.title === info.event.title && event.start === info.event.startStr
			);
			selectedEvent = { ...clickedEvent }; // Clonar el evento seleccionado
			showEventModal = true;
		  },
		});
		calendar.render();
	  }
	};
  
	// Destruir FullCalendar
	const destroyCalendar = () => {
	  if (calendar) {
		calendar.destroy();
		calendar = null;
	  }
	};
  
	// Reactivo para manejar cambios de vista
	$: if (view === "Mes") {
	  initCalendar();
	} else {
	  destroyCalendar();
	}
  
	// Añadir un nuevo evento
	const addEvent = () => {
	  if (newEventTitle.trim()) {
		const newEvent = {
		  title: newEventTitle,
		  start: format(selectedDate, "yyyy-MM-dd"),
		  description: newEventDescription,
		};
		events = [...events, newEvent]; // Forzar reactividad con una nueva copia del arreglo
		saveEventsToLocalStorage(); // Guardar en localStorage
		if (calendar) {
		  calendar.addEvent(newEvent); // Reflejar el evento en FullCalendar
		}
	  }
	  // Limpiar formulario y cerrar modal
	  newEventTitle = "";
	  newEventDescription = "";
	  showModal = false;
	};
  
	// Actualizar un evento
	const updateEvent = () => {
	  const index = events.findIndex((event) => event.title === selectedEvent.title && event.start === selectedEvent.start);
	  if (index !== -1) {
		events[index] = { ...selectedEvent }; // Actualizar el evento en el arreglo
		saveEventsToLocalStorage(); // Guardar en localStorage
  
		// Reflejar cambios en FullCalendar
		if (calendar) {
		  calendar.getEvents().forEach((fcEvent) => {
			if (fcEvent.title === selectedEvent.title && fcEvent.startStr === selectedEvent.start) {
			  fcEvent.setProp("title", selectedEvent.title);
			}
		  });
		}
	  }
	  showEventModal = false;
	};
  
	// Eliminar un evento
	const deleteEvent = () => {
	  events = events.filter((event) => !(event.title === selectedEvent.title && event.start === selectedEvent.start));
	  saveEventsToLocalStorage(); // Guardar en localStorage
  
	  // Eliminar el evento de FullCalendar
	  if (calendar) {
		calendar.getEvents().forEach((fcEvent) => {
		  if (fcEvent.title === selectedEvent.title && fcEvent.startStr === selectedEvent.start) {
			fcEvent.remove();
		  }
		});
	  }
	  showEventModal = false;
	};
  
	// Cambiar al día anterior
	const prevDay = () => {
	  const newDate = subDays(selectedDate, 1);
	  onDateChange(newDate);
	};
  
	// Cambiar al día siguiente
	const nextDay = () => {
	  const newDate = addDays(selectedDate, 1);
	  onDateChange(newDate);
	};
  
	// Obtener eventos del día seleccionado
	const eventsForSelectedDay = () =>
	  events.filter((event) => format(new Date(event.start), "yyyy-MM-dd") === format(selectedDate, "yyyy-MM-dd"));
  </script>
  
  <div class="calendar-container">
	{#if view === "Mes"}
	  <div bind:this={calendarEl}></div> <!-- FullCalendar se renderiza aquí -->
	{:else if view === "Día"}
	  <div class="day-view">
		<h2>Vista del día</h2>
		<div class="day-navigation">
		  <button on:click={prevDay}>⬅️</button>
		  <div class="selected-day">{format(selectedDate, "EEEE, dd MMMM yyyy")}</div>
		  <button on:click={nextDay}>➡️</button>
		</div>
		<div class="day-add-event">
		  <button on:click={() => (showModal = true)}>➕ Añadir Evento</button>
		</div>
		<div class="day-events">
		  <h3>Eventos:</h3>
		  {#if eventsForSelectedDay().length > 0}
			<div class="event-cards">
			  {#each eventsForSelectedDay() as event}
				<div class="event-card">
				  <div class="event-info">
					<h4>{event.title}</h4>
					<p>{event.description}</p>
				  </div>
				  <div class="event-actions">
					<button class="edit-button" on:click={() => { selectedEvent = { ...event }; showEventModal = true; }}>✏️ Editar</button>
					<button class="delete-button" on:click={() => { selectedEvent = { ...event }; deleteEvent(); }}>🗑️ Eliminar</button>
				  </div>
				</div>
			  {/each}
			</div>
		  {:else}
			<p>No hay eventos para este día.</p>
		  {/if}
		</div>
	  </div>
	{/if}
  </div>
  
  <!-- Modal para añadir eventos -->
  {#if showModal}
	<div class="modal-overlay">
	  <div class="modal">
		<h2>Nuevo Evento</h2>
		<label for="title">Título del Evento:</label>
		<input
		  id="title"
		  type="text"
		  bind:value={newEventTitle}
		  placeholder="Ingresa el título del evento"
		/>
		<label for="description">Descripción:</label>
		<textarea
		  id="description"
		  bind:value={newEventDescription}
		  placeholder="Ingresa la descripción del evento"
		></textarea>
		<div class="modal-actions">
		  <button on:click={addEvent}>Guardar</button>
		  <button on:click={() => (showModal = false)}>Cancelar</button>
		</div>
	  </div>
	</div>
  {/if}
  
  <!-- Modal para ver/editar un evento -->
  {#if showEventModal}
	<div class="modal-overlay">
	  <div class="modal">
		<h2>Editar Evento</h2>
		<label for="edit-title">Título del Evento:</label>
		<input
		  id="edit-title"
		  type="text"
		  bind:value={selectedEvent.title}
		  placeholder="Editar el título del evento"
		/>
		<label for="edit-description">Descripción:</label>
		<textarea
		  id="edit-description"
		  bind:value={selectedEvent.description}
		  placeholder="Editar la descripción del evento"
		></textarea>
		<div class="modal-actions">
		  <button on:click={updateEvent}>Guardar Cambios</button>
		  <button on:click={deleteEvent}>Eliminar Evento</button>
		  <button on:click={() => (showEventModal = false)}>Cancelar</button>
		</div>
	  </div>
	</div>
  {/if}
  
  <style>
	.calendar-container {
	  padding: 1rem;
	  background-color: #ffffff;
	  flex-grow: 1;
	  overflow-y: auto;
	}
  
	.day-view {
	  text-align: center;
	  padding: 1rem;
	}
  
	.day-navigation {
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  gap: 1rem;
	}
  
	.day-navigation button {
	  background-color: #007bff;
	  color: white;
	  border: none;
	  padding: 0.5rem 1rem;
	  font-size: 1rem;
	  border-radius: 8px;
	  cursor: pointer;
	}
  
	.day-navigation button:hover {
	  background-color: #0056b3;
	}
  
	.selected-day {
	  font-size: 1.25rem;
	  color: #007bff;
	  background-color: #f0f0f0;
	  padding: 1rem;
	  border-radius: 8px;
	}
  
	.day-add-event button {
	  background-color: #28a745;
	  color: white;
	  border: none;
	  padding: 0.5rem 1rem;
	  font-size: 1rem;
	  border-radius: 8px;
	  cursor: pointer;
	  margin-top: 1rem;
	}
  
	.day-add-event button:hover {
	  background-color: #218838;
	}
  
	.modal-overlay {
	  position: fixed;
	  top: 0;
	  left: 0;
	  width: 100%;
	  height: 100%;
	  background: rgba(0, 0, 0, 0.5);
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  z-index: 10;
	}
  
	.modal {
	  background: #fff;
	  padding: 2rem;
	  border-radius: 8px;
	  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	  width: 400px;
	  max-width: 90%;
	  text-align: center;
	}
  
	.modal-actions {
	  display: flex;
	  justify-content: space-around;
	}
  </style>
  