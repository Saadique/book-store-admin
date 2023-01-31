<template>
  <div class="container">
    <div v-bind:class="modalClass" class="modal-container">
      <div class="user-modal">
        <h3 class="text-info">Edit Book</h3>
        <form>
          <div class="form-group">
            <label for="titleEdit">Title</label>
            <input v-bind:value="book.title" required type="text" id="titleEdit" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="">Author</label>
            <input v-bind:value="book.author" required type="text" id="authorEdit" class="form-control"/>
          </div>
  
          <div class="form-group">
            <label for="">ISBN</label>
            <input v-bind:value="book.isbn" required type="text" id="isbnEdit" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="">Genre</label>
            <input v-bind:value="book.genre" required type="text" id="genreEdit" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="">Published</label>
            <input v-bind:value="book.published" required type="date" id="publishedEdit" class="form-control"/>
          </div>

          <div class="form-group">
            <label for="">Description</label>
            <textarea v-bind:value="book.description" required id="descriptionEdit" class="form-control"/>
          </div>
          

          <button v-on:click.prevent="showModal = !showModal" class="btn btn-secondary mr-1">Cancel</button>
          <button v-on:click="updateBook(book.id)" type="submit" class="btn btn-primary">Save</button> 
        </form>
      </div>
    </div>

  </div>
</template>

<script>
import { EventBus } from '../main.js';

export default {
  name: 'EditBookModal',
  data() {
    return {
      books: [],
      book: '',
      showModal: false
    }
  },
  methods: {
    async updateBook(id) {
      await fetch(`http://127.0.0.1:8000/api/books/${id}`, {
        method: 'PUT',
        headers: { 'Content-type': 'application/json' },
        body: JSON.stringify({ 
          title: document.getElementById('titleEdit').value,
          author: document.getElementById('authorEdit').value,
          isbn: document.getElementById('isbnEdit').value,
          genre: document.getElementById('genreEdit').value,
          published: document.getElementById('publishedEdit').value,
          description: document.getElementById('descriptionEdit').value
        }),
      })
      .then((response) => {response.json(); console.log(response)})
      .then(() => {
        this.fetchData();
      })
      .catch((err) => {
        console.error(err);
      });
    }
  },
  
  created() {
    EventBus.$on('click', (modal, book) => {
      this.showModal = !modal;
      this.book = book;
      console.log('showModal', this.showModal);
      console.log('modal', modal)
    })
  },
  async fetchData(){
        await fetch('http://127.0.0.1:8000/api/admin/books')
        .then(response => response.json())
        .then(data => {
          this.books = data.data;
        })
        .catch(err => {
          console.error(err);
        });
    },
  
  computed: {
    modalClass() {
      return this.showModal ? 'show' : '';
    }
  }
  
};
</script>

<style>

</style>
