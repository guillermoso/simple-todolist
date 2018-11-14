<template>
  <div id="app">
    <todolist></todolist>
  </div>
</template>

<script>
import todolist from './components/todo-list/list-header.vue'

export default {
  name: 'App',
  components: {
    todolist
  }
}
</script>

<style>
#app {
  font-family: 'Merriweather Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: linear-gradient( #b3d1ff , #e6f0ff);
  height: 100vh;
  display: flex;
  justify-content: center;
}

body {
  margin: 0;
}

.blue {
    background-color: #000080;
}

.red {
    background-color: #cc0000;
}

.green {
    background-color: #00cc00;
}

</style>
