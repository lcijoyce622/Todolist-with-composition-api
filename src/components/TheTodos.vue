<template>
  <div class="cointainer"><new-todo></new-todo> <todo-list></todo-list></div>
</template>

<script>
import newTodo from "./newTodo.vue";
import todoList from "./todoList.vue";
import { ref, provide, onMounted } from "vue";

export default {
  components: {
    newTodo,
    todoList,
  },
  setup() {
    const todos = ref([]);
    function setHistory() {
      if (JSON.parse(localStorage.getItem("todoHistory")) != null) {
        const localTodos = JSON.parse(localStorage.getItem("todoHistory"));
        localTodos.forEach((todo) => todos.value.push(todo));
      }
    }
    function saveData() {
      localStorage.setItem("todoHistory", JSON.stringify(todos.value));
    }
    provide('todos',todos);
    provide('saveData',saveData);
        console.log(todos)

    onMounted(() => {
      setHistory();
    });
    return {
      todos,
      setHistory,
      saveData,
    };
  },
  // data() {
  //   return {
  //     todos: [],
  //   };
  // },
  // methods: {
  //   setHistory() {
  //     if (JSON.parse(localStorage.getItem("todoHistory")) != null) {
  //       const localTodos = JSON.parse(localStorage.getItem("todoHistory"));
  //       localTodos.forEach((todo) => this.todos.push(todo));
  //     }
  //   },
  //   saveData() {
  //     localStorage.setItem("todoHistory", JSON.stringify(this.todos));
  //   },
  // },
  // provide() {
  //   return {
  //     todos: this.todos,
  //     saveData: this.saveData,
  //   };
  // },
  // mounted() {
  //   this.setHistory();
  // },
};
</script>