<template>
  <div class="main">
    <form @submit.prevent="addTodo">
      <h2 for="newTodo">Add new todo element</h2>
      <div>
      <input placeholder="Enter new todo element here" type="text" name="newTodo" v-model="state.TodoElement">
      <button>Add</button>
      <div v-if="isInputEmpty">You need to fill this field to add todo element!</div>
      </div>
    </form>
      <ul class="todo-list">
        <div v-if="!state.TodosArray.length" class="todo-list__nothing-to-do">Congrats you have nothing to do! 🎉</div>
        <h2 v-if="state.TodosArray.length" class="todo-list__nothing-to-do">Here is your to-do list:</h2>
        <li v-for="(todo, index) in state.TodosArray" :key="todo.id" >
          <div class="todo-list__radio" :class="{ done: todo.done }" @click="toggleDone(todo)"></div>
          <span contenteditable="true" @input="handleExisitngTodoInput(todo,index)">{{todo.content}}</span>
          <button @click="removeTodo(index)">Delete this todo</button>
        </li>
      </ul>

      <div class="toggleButton" v-if="state.TodosArray.length" @click="toggleAllStates()">Toggle all done</div>
    
  </div>
</template>

<script>
import {reactive,computed} from 'vue';
export default {
  name: 'TodoInterface',
  setup() {
    const state = reactive({
      TodoElement: "",
      TodosArray: [],
      InputEmpty: null
    })
    const isInputEmpty = computed(() => state.InputEmpty)
    const checkArrayIfAllTrue = computed(() => state.TodosArray.every(e => e === true));
    function addTodo() {
      if (state.TodoElement) {
        state.TodosArray.push({
          id: Date.now(),
          done: false,
          content: state.TodoElement
        });
        state.TodoElement = "";
        state.InputEmpty = false;
      } else {
        state.InputEmpty = true;
      }
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(index) {
      state.TodosArray.splice(index, 1);
    }


    function handleExisitngTodoInput(todo,index){

      todo.content = event.target.innerText;
      if(todo.content == ""){
        state.TodosArray.splice(index, 1);
      }
    }

    function toggleAllStates() {
      //why it doesn't work????
      console.log(checkArrayIfAllTrue.value)
      if (checkArrayIfAllTrue.value) {
        state.TodosArray.forEach(element => {
          element.done = false;
        });
      } else {
        state.TodosArray.forEach(element => {
          element.done = true;
        });
      }
    }
    return {

      state,
      isInputEmpty,
      checkArrayIfAllTrue,
      addTodo,
      toggleDone,
      removeTodo,
      handleExisitngTodoInput,
      toggleAllStates
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.main{
  width: 40%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  font-family: Poppins;
    display: flex;
  flex-direction: column;
  form{
  @include component-base;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  h2{
    margin-bottom: 5px;
    
  }
  input[type=text]{
    padding: 10px 40px;
    text-align: center;
    border-radius: 5px;
    outline: none;
    border: none;
    border: 2px solid white;
    font-family: Poppins;
    font-size: .95em;
    &:focus{
      border: 2px solid $color800;
    }
  }
  button{
    padding: 15px 50px;
    border-radius: 5px;
    border: none;
    background-color: $color300;
    color: $color50;
    text-transform: uppercase;
    font-weight: 800;
    cursor: pointer;
    margin-left: 10px;
  }
  } 


ul {
  list-style: none;
  margin-top: 50px;
  width: 100%;
  background-color: $grey-component;
  @include component-base;
      .todo-list__nothing-to-do{
      font-size: 1.2em;
    }
  li{
    display: flex;
    flex-direction: row;
    width: 100%;

    .todo-list__radio{
      width:35px;
      height: 35px;
      border-radius: 50%;
      
      background-color: rgba(219, 178, 255, 0.3);
      cursor: pointer;
    }
    .done{
      background-color: $color800;
    }
  }
  span{
  cursor: text;
  }
}
.toggleButton{
  cursor: pointer;

}
}
</style>
