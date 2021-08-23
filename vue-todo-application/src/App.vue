<template>
  <h1>To Do App </h1>
  <TodoInterface />
</template>

<script>
import TodoInterface from './components/TodoInterface.vue'

export default {
  name: 'App',
  components: {
    TodoInterface
  }
}
</script>

<style lang="scss">
#app {
  h1{
    font-family: Poppins;
    text-transform: uppercase;
    font-size: 3em;
    text-shadow: 3px 3px #9465F7;

  }
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}
</style>
