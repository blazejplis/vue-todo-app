<template>
<div class="main">
  <div class="header">      
    <h1>Todo App</h1></div>
  <TodoInterface />
  </div>
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
  .header{
    margin-bottom: 50px;
  }
  h1{
    font-family: Poppins;
    text-transform: uppercase;
    font-size: 3em;
    text-shadow: 3px 3px #9465F7;
    background-color: $grey-component;
    padding: 20px 100px;
    border-radius: 10px;

  }
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}

</style>
