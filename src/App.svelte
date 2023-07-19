<script>
  import { onMount } from "svelte";
  import BookList from "./components/BookList.svelte";

  let readList = [];
  let books = [];
  let selectedGenre = "";
  let filteredBooks = [];

  // Cargar los datos del archivo JSON utilizando una función asincrónica.
  async function loadBooks() {
    try {
      const response = await fetch("/data/books.json");
      books = await response.json();
    } catch (error) {
      console.error("Error al cargar los datos de books.json", error);
    }
  }

  // Función para cambiar el estado de la lista de lectura
  function toggleReadList(id) {
    if (readList.includes(id)) {
      readList = readList.filter((bookId) => bookId !== id);
    } else {
      readList = [...readList, id];
    }

    // Guardamos la lista de lectura en el almacenamiento local
    saveReadList();
  }

  // Función para cargar la lista de lectura desde el almacenamiento local del navegador.
  function loadReadList() {
    const storedReadList = localStorage.getItem("readList");
    if (storedReadList) {
      readList = JSON.parse(storedReadList);
    }
  }

  // Función para guardar la lista de lectura en el almacenamiento local del navegador.
  function saveReadList() {
    localStorage.setItem("readList", JSON.stringify(readList));
  }

  function filterBooksByGenre(event) {
    selectedGenre = event.target.value;
  }

  // Cargar la lista de lectura cuando se monta el componente.
  onMount(() => {
    loadBooks();
    window.addEventListener("storage", handleStorageChange);
  });

  function handleStorageChange(event) {
    if (event.key === "readList") {
      readList = JSON.parse(event.newValue || "[]");
    }
  }

  $: {
    loadReadList();
    filteredBooks = selectedGenre
      ? books.filter((book) => book.genre === selectedGenre)
      : books;
  }
</script>

<main>
  <h3>Filtrar por Género</h3>
  <select bind:value={selectedGenre} on:change={filterBooksByGenre}>
    <option value="">Todos los géneros</option>
    {#each [...new Set(books.map((book) => book.genre))] as genre}
      <option value={genre}>{genre}</option>
    {/each}
  </select>

  <BookList books={filteredBooks} {toggleReadList} {readList} {selectedGenre} />
  <h2>Lista de Lectura</h2>
  {#if readList.length === 0}
    <p>No hay libros en la lista de lectura.</p>
  {:else}
    <ul>
      {#each books as book}
        {#if readList.includes(book.id)}
          <li>{book.title}</li>
        {/if}
      {/each}
    </ul>
  {/if}
</main>

<style>
  main {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
  }
</style>
