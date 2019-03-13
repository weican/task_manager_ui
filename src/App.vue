<template>
  <div id="app">
    <TaskManager msg="Welcome to Task Manager App"/>
  </div>
</template>

<script>
import TaskManager from './components/TaskManager.vue'

export default {
  name: 'app',
  components: {
    TaskManager
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
