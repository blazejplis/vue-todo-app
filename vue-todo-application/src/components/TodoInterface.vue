<template>
  <div class="main">
    <form @submit.prevent="addTodo">
      <label for="newTodo">Add new to-do element</label>
      <input type="text" name="newTodo" v-model="TodoElement">

      <button>Add</button>
      <br>
      <h2>{{ TodoElement }}</h2>
    </form>
  </div>
</template>

<script>
export default {
  name: 'TodoInterface',
  setup(){
      
    let TodoElement = "";
    function addTodo(){
      console.log("Form submitted");
    }

      return{
        TodoElement,
        addTodo,
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
.main{
  width: 40%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
}
.main form{
  width: 100%;
  display: flex;
  flex-direction: column;
}
</style>
