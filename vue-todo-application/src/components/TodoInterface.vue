<template>
  
    <form @submit.prevent="addTodo">
      <h2 for="newTodo">Add new todo element</h2>
      <div>
      <input placeholder="Enter new todo element here" type="text" name="newTodo" spellcheck="false" v-model="state.TodoElement">
      <button>Add</button>
      <div v-if="isInputEmpty">You need to fill this field to add todo element!</div>
      </div>
    </form>
      <ul class="todo-list">
        <div v-if="!state.TodosArray.length" class="todo-list__nothing-to-do">Congrats you have nothing to do! 🎉</div>
        <div v-if="state.TodosArray.length" class="todo-list__table-header"><h2>Check/uncheck</h2><h2>Task to do<span>(editable)</span></h2><h2>Remove task</h2></div>
        <li v-for="(todo, index) in state.TodosArray" :key="todo.id" >
          <div class="todo-list__radio" :class="{ done: todo.done }" @click="toggleDone(todo)"></div>
          <span contenteditable="true" @input="handleExisitngTodoInput(todo,index)">{{todo.content}}</span>
          <button @click="removeTodo(index)">Delete</button>
        </li>
      </ul>
      <button class="toggleButton" v-if="state.TodosArray.length" @click="toggleAllStates(state.TodosArray)">Toggle all done</button>
      <transition name="fade">
      <div class="successModal" v-if="checkArrayIfAllTrue && state.ModalSeen === false" ><h3>You managed to finish all tasks! 🥳</h3><span v-on:click="state.ModalSeen = true">Hide</span></div>
      </transition>
</template>

<script>
import {reactive,computed} from 'vue';
export default {
  name: 'TodoInterface',
  setup() {
    const state = reactive({
      TodoElement: "",
      TodosArray: [],
      InputEmpty: null,
      ModalSeen: false
    })
    const isInputEmpty = computed(() => state.InputEmpty);
    const checkArrayIfAllTrue = computed(() => {
      let result = false;
      for (let i = 0; i < state.TodosArray.length; i++) {
          if (state.TodosArray[i].done === true) {
              result = true;
          }
      }
      return result;
    });
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

    function toggleAllStates(array) {
      //why it doesn't work????

      if (checkArrayIfAllTrue.value) {
        array.forEach(element => {
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
<style  lang="scss">
.main {
  width: 40%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  font-family: Poppins;
  display: flex;
  flex-direction: column;
  

  form {
    @include component-base;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    h2 {
      margin-bottom: 5px;

    }

    input[type=text] {
      padding: 10px 40px;
      text-align: center;
      border-radius: 5px;
      outline: none;
      border: none;
      border: 2px solid white;
      font-family: Poppins;
      font-size: .95em;
      height: 100%;

      &:focus {
        border: 2px solid $color800;
      }
    }

    button {
      padding: 15px 50px;
      margin-left: 20px;
      @include button-base;
    }
  }


  ul {
    list-style: none;
    margin-top: 50px;
    width: 100%;
    background-color: $grey-component;
    @include component-base;
    .todo-list{
          .todo-list__nothing-to-do {
      font-size: 1.2em;
    }

    }
      .todo-list__table-header{
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        width: 100%;
        border-bottom: 1px solid white;
        padding-bottom: 10px;
        margin-bottom: 30px;
        justify-content: baseline;
        align-items: baseline;

        h2{
          font-size: 1.2em;
          text-align: center;
          display: flex;
          flex-direction: column;
          height: 100%;
          span{
            font-size: .7em;
            opacity: .7;
            font-weight: 500;
          }
        }
      }

    li {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      align-items: center;
      width: 100%;
      margin: 10px 0;

      .todo-list__radio {
        width: 40px;
        height: 40px;
        border-radius: 50%;
        border: 4px solid rgba(219, 178, 255, 1);
        background-color: rgba(219, 178, 255, 0.1);
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        justify-self: center;
        &:hover{
          background-color: rgba(219, 178, 255, 0.6);
        }
      }

      .done {
        background-color: rgba(219, 178, 255, 1);
      }
      span{
        padding: 5px;
        max-width: 100%;
        justify-self: center;
      }
      button {
        @include button-base;
        padding: 10px 20px;
        justify-self: center;
        font-weight: 600;
        background-color: transparent;
        border: 3px solid $color300;
        &:hover{
          background-color: $color200;

        }
      }
    }

    span {
      cursor: text;
    }
  }

  .toggleButton {
    @include button-base;
    cursor: pointer;
    padding: 10px 20px;
    margin-top: 40px;

  }
}
.successModal{
  position: fixed;
  width: 100vw;
  height: 100vh;
  background-color: rgba(18, 18, 18,0.9);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  h3{
    font-size: 3rem;
  }
  span{
    cursor: pointer;
    text-decoration: underline;
    margin-top: 50px;
    text-transform: lowercase;
  }
}
</style>
