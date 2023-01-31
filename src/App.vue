<template>
  <div id="app">
    <Header />
    <CreateBookModal />
    <EditBookModal />
    <BooksTable />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import CreateBookModal from './components/CreateBookModal.vue';
import EditBookModal from './components/EditBookModal.vue';
import BooksTable from './components/BooksTable.vue';

export default {
  name: 'App',
  components: {
    Header,
    CreateBookModal,
    BooksTable,
    EditBookModal
  } 
}
</script>

<style>
#app {
  margin-top: 40px;
}
</style>
