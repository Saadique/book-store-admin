<template>
  <div class="container">
    <h3 class="mt-2 mb-3 float-left text-primary">Books List</h3>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">Number</th>
          <th scope="col">Title</th>
          <th scope="col">Author</th>
          <th scope="col">ISBN</th>
          <th scope="col">Genre</th>
          <th scope="col">Published</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(book, index) in books" :key="book.id" class="m-5">
          <th scope="row">{{ index + 1 }}</th>
          <td>{{ book.title }}</td>
          <td>{{ book.author }}</td>
          <td>{{ book.isbn }}</td>
          <td>{{ book.genre }}</td>
          <td>{{ book.published }}</td>

          <td>
            <form>
              <button v-on:click.prevent="emitShowModal(book)" class="btn btn-sm btn-info mr-1">Edit</button>
              <button v-on:click.prevent="deleteBook(book.id)" class="btn btn-sm btn-danger">Delete</button>
            </form>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { EventBus } from '../main.js';

export default {
  name: 'BooksTable',
  data() {
    return {
      books: [],
      showModal: false
    }
  },
  
  async created() {
    this.fetchData()
  },
  methods: {
    async deleteBook(id) {
      await fetch(`http://127.0.0.1:8000/api/books/${id}`, {
        method: 'DELETE'
      })
      .then((response) => {response.json(); this.fetchData()})
      .catch((err) => {
        console.error(err);
      });
    },
    
    emitShowModal(book) {
      this.book = book;
      EventBus.$emit('click', this.showModal, this.book);
      
      console.log(this.book);

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
    }
  }
}
</script>

<style scoped>
th {
  padding-left: 1.2rem;
}
</style>
