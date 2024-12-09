<script>
    export let isOpen = false;
    export let title = '';
    export let product = { nombre: '', precio: 0, stock: 0 };
    export let onSubmit;
  
    const handleSubmit = () => {
      onSubmit(product);
      isOpen = false;
    };
  
    const closeModal = (event) => {
      if (event.target.classList.contains('overlay')) {
        isOpen = false;
      }
    };
  </script>
  
  <style>
    .overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 999;
    }
  
    .modal {
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
      width: 90%;
      max-width: 400px;
    }
  
    .buttons {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      margin-top: 20px;
    }
  </style>
  
  {#if isOpen}
  <div
    class="overlay"
    on:click={closeModal}
    tabindex="-1"   
    aria-hidden="true"
  >
    <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
    <div
      class="modal"
      on:click|stopPropagation
      on:keydown={(e) => {
        if (e.key === "Escape") isOpen = false; 
      }}
      tabindex="0"  
      aria-labelledby="modal-title"
      role="dialog"
      aria-modal="true"  
    >
      <h2 id="modal-title">{title}</h2>
  
      <label for="product-name">
        Nombre:
        <input
          id="product-name"
          type="text"
          bind:value={product.nombre}
          placeholder="Introduce el nombre del producto"
          aria-label="Nombre del producto"
        />
      </label>
  
      <label for="product-price">
        Precio:
        <input
          id="product-price"
          type="number"
          bind:value={product.precio}
          placeholder="Introduce el precio del producto"
          aria-label="Precio del producto"
          min="0"  
        />
      </label>
  
      <label for="product-stock">
        Stock:
        <input
          id="product-stock"
          type="number"
          bind:value={product.stock}
          placeholder="Introduce el stock del producto"
          aria-label="Stock del producto"
          min="0"  
        />
      </label>
  
      <div class="buttons">
        <button
          on:click={handleSubmit}
          aria-label="Guardar producto"
        >
          Guardar
        </button>
        <button
          on:click={() => (isOpen = false)}
          aria-label="Cancelar acción"
        >
          Cancelar
        </button>
      </div>
    </div>
  </div>
  {/if}
  