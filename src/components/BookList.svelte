<script>
  import Book from "./Book.svelte";

  export let books;
  export let toggleReadList;
  export let readList = [];
  export let selectedGenre = "";

  function countAvailableBooksByGenre(genre) {
    return books.filter((book) => {
      return book.genre === genre && !readList.includes(book.id);
    }).length;
  }

  function countReadListBooksByGenre(genre) {
    return readList.filter((id) => {
      return books.find((book) => book.id === id)?.genre === genre;
    }).length;
  }
</script>

<div>
  <h2>Libros Disponibles</h2>
  {#each books as book}
    <Book {book} inReadList={readList.includes(book.id)} {toggleReadList} />
  {/each}

  <div>
    <h3>{selectedGenre || "Todos"}</h3>
    <p>
      Libros disponibles:
      {selectedGenre ? countAvailableBooksByGenre(selectedGenre) : books.length - readList.length}
    </p>
    <p>
      Libros en la lista de lectura:
      {selectedGenre ? countReadListBooksByGenre(selectedGenre) : readList.length}
    </p>
  </div>
</div>
