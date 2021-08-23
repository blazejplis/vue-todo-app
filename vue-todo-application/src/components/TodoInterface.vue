<template>
  <div class="main">
    <form @submit.prevent="addTodo">
      <label for="newTodo">Add new todo element</label>
      <input placeholder="Enter new todo element here" type="text" name="newTodo" v-model="state.TodoElement">
      <button>Add</button>
    </form>
      <ul class="todo-list">
        <li v-for="(todo, index) in state.TodosArray" :key="todo.id" >
          <div class="todo-list__radio" :class="{ done: todo.done }" @click="toggleDone(todo)"></div>
          <span contenteditable="true">{{todo.content}}</span>
          <button @click="removeTodo(index)">Delete this todo</button>
        </li>
      </ul>

      <div class="toggleButton" v-if="state.TodosArray.length" @click="toggleAllStates()">Toggle all done</div>
    
  </div>
</template>

<script>
import { reactive } from 'vue';
export default {
  name: 'TodoInterface',
  setup(){
    const state = reactive({
        TodoElement: "",
        TodosArray: [],
    })

    function addTodo(){
      if(state.TodoElement){
      state.TodosArray.push({
        id: Date.now(),
        done: false,
        content: state.TodoElement
      });
      state.TodoElement = "";
      }
    }
      function toggleDone(todo){
          todo.done = !todo.done;
      }
      function removeTodo(index){
          state.TodosArray.splice(index,1);
      }
      
      function toggleAllStates(){
        //why it doesn't work????
        console.log(state.TodosArray.every(e => e))
        if(state.TodosArray.every(e => e)){
          state.TodosArray.forEach(element => {
              element.done = false ;
          });
        }
        else{
          state.TodosArray.forEach(element => {
              element.done = true;
          });
        }
      }
      return{
        state,
        addTodo,
        toggleDone,
        removeTodo,
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
  width: 100%;
  padding: 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #1d1d1d;
  border-radius: 10px;
  input[type=text]{
    padding: 10px;
    border-radius: 5px;
    outline: none;
    border: none;
    border: 2px solid white;
    font-family: Poppins;
    font-size: .95em;
    &:focus{
      border: 2px solid #30009c;
    }
  }
  button{
    padding: 15px 20px;
    
  }
  } 
}

ul {
  list-style: none;
  width: 100%;
  li{
    display: flex;
    flex-direction: row;
    width: 100%;
    .todo-list__radio{
      width:25px;
      height: 25px;
      border-radius: 50%;
      border: 1px solid #7A3EFA;
      background-color: rgba(210, 255, 40,0.4);
      cursor: pointer;
    }
    .done{
      background-color: rgba(210, 255, 40,1);
    }
  }
  span{
  cursor: text;
  }
}
.toggleButton{
  cursor: pointer;

}

</style>
