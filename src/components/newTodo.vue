<template>
  <h1>Vue3 ToDo App with Composition api</h1>
  <form @submit.prevent="addNewTodo">
    <!-- <label for="addTodo">New ToDo</label> -->
    <input
      type="text"
      name="addTodo"
      v-model="newTodo"
      placeholder="請輸入代辦事項"
    />
    <button>新增代辦事項</button>
  </form>
</template>

<script>
import { ref, inject } from "vue";
export default {
  
  setup() {
    const todos = inject('todos');
    const saveData = inject('saveData');
    const newTodo = ref("");
    function addNewTodo() {
      if (newTodo.value.trim() == "") {
        return;
      }
      todos.value.unshift({
        id: todos.value.length === 0 ? 0 : todos.value[0].id + 1,
        done: false,
        content: newTodo.value,
        isEdit: false,
      });
      console.log(todos.value);
      newTodo.value = "";
      saveData();
    }

    return {
      todos,
      newTodo,
      addNewTodo,
      saveData
    };
  },
  // data() {
  //   return {
  //     newTodo: "",
  //   };
  // },
  // methods: {
  //   addNewTodo() {
  //     if (this.newTodo.trim() == "") {
  //       return;
  //     }
  //     console.log(this.todos[0]);
  //     this.todos.unshift({
  //       id: this.todos.length===0?0:this.todos[0].id + 1,
  //       done: false,
  //       content: this.newTodo,
  //       isEdit:false,
  //     });
  //     console.log(this.todos);
  //     this.newTodo = "";
  //     this.saveData();
  //   },
  // },
};
</script>


<style lang="scss" scope>
h1 {
  text-align: center;
}
form {
  display: flex;
  flex-direction: column;
  width: 100%;
  label {
    font-weight: bold;
  }
  input,
  button {
    padding: 1rem;
    margin: 0.5rem 0;
    border: 2px solid rgb(92, 168, 255);
    border-radius: 5px;
    outline: none;
  }
  input {
    background-color: #1f2937;
    color: rgb(255, 255, 255);
  }
  button {
    background-color: rgb(92, 168, 255);
    font-size: 18px;
    font-weight: bold;
    border: none;
  }
}
</style>