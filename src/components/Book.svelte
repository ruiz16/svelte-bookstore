<script>
  export let book;
  export let inReadList;
  export let toggleReadList;
</script>

<div class="book">
  <h3>{book.title}</h3>
  <p>Author: {book.author}</p>
  <p>Genre: {book.genre}</p>
  <p>Pages: {book.pages}</p>
  <button on:click={() => toggleReadList(book.id)}>
    {inReadList
      ? "Remover de la Lista de Lectura"
      : "Agregar a la Lista de Lectura"}
  </button>
</div>

<style>
  .book {
    border: 1px solid #ccc;
    padding: 10px;
    margin: 10px;
    border-radius: 5px;
  }
</style>
