<template>
  <div class="container">
    <button v-on:click="showModal = !showModal" class="btn btn-lg btn-outline-primary float-right">Add Book</button>
    
    <div v-bind:class="modalClass" class="modal-container">
      <div class="user-modal">
        <h3 class="text-primary">Add New Book</h3>
        <form>
          <div class="form-group">
            <label for="title">Title</label>
            <input required value="" type="text" id="title" class="form-control"/>
          </div>
    
          <div class="form-group">
            <label for="author">Author</label>
            <input required value="" type="text" id="author" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="isbn">ISBN</label>
            <input required value="" type="text" id="isbn" class="form-control"/>
          </div>
          
          <div class="form-group">
            <label for="genre">Genre</label>
            <input required value="" type="text" id="genre" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="published">Published</label>
            <input required value="" type="date" id="published" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="description">Description</label>
            <textarea required value="" id="description" class="form-control"/>
          </div>
          
          <button v-on:click.prevent="showModal = !showModal" class="btn btn-secondary mr-1">Cancel</button>
          <button v-on:click="createBook()" type="submit" class="btn btn-primary">Save</button> 
        </form>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'CreateBookModal',
  data() {
    return {
      books: [],
      showModal: false,
    };
  },
  methods: {
    // Requisição POST para criar um novo usuário
    async createBook() {
      await fetch('http://127.0.0.1:8000/api/books/', {
        method: 'POST',
        headers: { 'Content-type': 'application/json' },
        body: JSON.stringify({
          title: document.getElementById('title').value,
          author: document.getElementById('author').value,
          isbn: document.getElementById('isbn').value,
          genre: document.getElementById('genre').value,
          published: document.getElementById('published').value,
          description: document.getElementById('description').value
        }),
      })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
      })
      .catch((err) => {
        console.error(err);
      });
    }
  },
  // Função para colocar ou tirar a classe "show" no Modal
  computed: {
    modalClass() {
      return this.showModal ? 'show' : '';
    }
  }
};
</script>

<style>
.modal-container {
  position: fixed;
  background-color: rgba(0, 0, 0, 0.3);
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;

  display: flex;
  align-items: center;
  justify-content: center;
  
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease-in-out;
}

.modal-container.show {
  opacity: 1;
  pointer-events: auto;
}

.user-modal {
  color: #535353;
  background-color: #fff;
  background-image: url("../assets/background.jpg");
  width: 450px;
  padding: 40px 50px;
  max-width: 100%;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.user-modal h1 {
  margin: 0;
}

.user-modal p {
  opacity: 0.9;
}
</style>
