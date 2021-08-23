<template>

    <form @submit.prevent="addTodo">
      <h2 for="newTodo">Add new todo element</h2>
      <div>
      <input placeholder="Enter new todo element here" type="text" name="newTodo" spellcheck="false" v-model="state.TodoElement">
      <button>Add</button>
      <div v-if="isInputEmpty">You need to fill this field to add todo element!</div>
      </div>
    </form>

</template>

<script>
import {reactive,computed} from 'vue';
export default {
  name: 'AddElement',
  setup(){
    const state = reactive({
      TodoElement: "",
    })
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
    const isInputEmpty = computed(() => state.InputEmpty);
    return{
        addTodo,
        isInputEmpty,
        state,
    }
  }
}
</script>

<style lang="scss">


</style>
