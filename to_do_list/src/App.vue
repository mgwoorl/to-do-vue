<template>
  <div id="app">
    <h1>ToDo App</h1>
    <to-do></to-do>
  </div>
</template>

<script>
import ToDo from './components/ToDo.vue';

export default {
  components: {
    ToDo
  }
};
</script>

<style>
body {
    font-family: Arial, sans-serif;
    background-color: #f9f9f9;
    margin: 0;
    padding: 20px;
}

h1 {
    text-align: center;
}
</style>