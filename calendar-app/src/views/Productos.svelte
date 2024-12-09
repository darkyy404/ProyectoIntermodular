<script>
  import ProductCard from '../components/ProductCard.svelte';
  import ProductModal from '../components/ProductModal.svelte';

  let searchQuery = '';
  let productos = [
    { id: 1, nombre: 'Producto 1', precio: 19.99, stock: 10 },
    { id: 2, nombre: 'Producto 2', precio: 5.99, stock: 50 },
  ];

  let filteredProducts = productos;
  let showModal = false;
  let modalProduct = { id: null, nombre: '', precio: 0, stock: 0 };

  const filterProducts = () => {
    filteredProducts = productos.filter(producto =>
      producto.nombre.toLowerCase().includes(searchQuery.toLowerCase())
    );
  };

</script>

<div class="container">
  <div class="header">
    <h1>Productos</h1>
  </div>
  <div class="search-bar">
    <input type="text" bind:value={searchQuery} placeholder="Buscar productos..." on:input={filterProducts} />
  </div>
  <div class="list">
    {#each filteredProducts as producto}
      <ProductCard {producto} />
    {/each}
  </div>
</div>

<style>
  /* Estilo general del contenedor */
  .container {
    max-width: 90%;
    margin: 0 auto;
    padding: 20px;
    background: white;
    border-radius: 16px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }

  /* Cabecera con el título y el botón de agregar */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  .header h1 {
    font-size: 1.5rem;
    color: #333;
  }

  .header button {
    background: #e4f4ff;
    border: none;
    color: #4a90e2;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    cursor: pointer;
  }

  /* Barra de búsqueda */
  .search-bar {
    display: flex;
    margin-bottom: 20px;
    gap: 10px;
  }

  .search-bar input {
    flex-grow: 1;
    padding: 10px;
    font-size: 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    outline: none;
  }

  .search-bar input:focus {
    border-color: #4a90e2;
  }

  /* Lista de productos */
  .list {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  @media (min-width: 768px) {
    .list {
      flex-direction: column;
    }
  }

  @media (min-width: 1024px) {
    .list {
      flex-direction: column;
    }
  }
</style>
