<template>
  <div class="list_title row justify-content-between align-items-end">
    <h5 class="col">你還有{{ undoneCount }}項事情沒做</h5>
    <div class="col filter_style">
      <button class="filter_style" :class="allFilter" @click="setFilter('all')">
        ALL
      </button>
      <button
        class="filter_style"
        :class="doneFilter"
        @click="setFilter('done')"
      >
        Done
      </button>
      <button
        class="filter_style"
        :class="undoneFilter"
        @click="setFilter('undone')"
      >
        UNDO
      </button>
    </div>
  </div>

  <p v-if="todos.length == 0">Please add some todos.</p>
  <ul v-else>
    <li
      v-for="todo in doneTodos"
      :key="todo.id"
      :class="{ donestyle: todo.done }"
    >
      <img src="../assets/drag.svg" alt="" />
      <span @click="newDone(todo.id)">
        <div :id="todo.id" v-if="todo.isEdit">
          <input :id="todo.id" type="text" v-model="newValue" />
        </div>
        <div v-else>
          <input type="checkbox" :id="todo.id" v-model="todo.done" />
          {{ todo.content }}
        </div>
      </span>
      <div>
        <button v-if="!todo.isEdit" @click="editTodo(todo.id)">Edit</button>
        <button v-else @click="sendEditTodo(todo.id)">OK</button>
        <button @click="removeTodo(todo.id)">Remove</button>
      </div>
    </li>
  </ul>
</template>

<script>
import { ref, inject, computed } from "vue";

export default {
  setup() {
    const todos = inject("todos");
    const saveData = inject("saveData");
    const filterTodos = ref("");
    const selectedFilter = ref("all");
    const newValue = ref("");

    function setFilter(filterTab) {
      selectedFilter.value = filterTab;
      if (filterTab === "done") {
        filterTodos.value = true;
        return;
      }
      if (filterTab === "undone") {
        filterTodos.value = false;
        return;
      }
      if (filterTab === "all") {
        filterTodos.value = "";
        return;
      }
    }
    function editTodo(todoId) {
      const idx = todos.value.findIndex((idx) => idx.id === todoId);
      todos.value[idx].isEdit = true;
      newValue.value = todos.value[idx].content;
    }
    function sendEditTodo(todoId) {
      const idx = todos.value.findIndex((idx) => idx.id === todoId);
      if (newValue.value != "" && newValue.value != todos.value[idx].content) {
        todos.value[idx].content = newValue.value;
        todos.value[idx].done = false;
      }
      console.log(todos.value[idx].content);
      todos.value[idx].isEdit = false;
      newValue.value = "";
      saveData();
    }
    function removeTodo(todoId) {
      const idx = todos.value.findIndex((idx) => idx.id === todoId);
      todos.value.splice(idx, 1);
      saveData();
    }
    function newDone(todoId) {
      const idx = todos.value.findIndex((idx) => idx.id === todoId);
      todos.value[idx].done = !todos.value[idx].done;
      saveData();
    }

    const doneTodos = computed(() => {
      return filterTodos.value === ""
        ? todos.value
        : todos.value.filter((item) => item.done === filterTodos.value);
    });
    const undoneCount = computed(() => {
      const doneTodos = todos.value.filter((item) => item.done === false);
      return doneTodos.length;
    });
    const allFilter = computed(() => {
      return selectedFilter.value == "all" ? "active" : null;
    });
    const doneFilter = computed(() => {
      return selectedFilter.value == "done" ? "active" : null;
    });
    const undoneFilter = computed(() => {
      return selectedFilter.value == "undone" ? "active" : null;
    });

    return {
      todos,
      saveData,
      filterTodos,
      selectedFilter,
      newValue,
      setFilter,
      editTodo,
      sendEditTodo,
      removeTodo,
      newDone,
      doneTodos,
      undoneCount,
      allFilter,
      doneFilter,
      undoneFilter
    };
  },

  // inject: ["todos", "saveData"],
  // data() {
  //   return {
  //     filterTodos: "",
  //     selectedFilter: "all",
  //     newValue: "",
  //     // editValue: false,
  //   };
  // },

  // computed: {
  //   // editValue(){
  //   //   return  this.isEdit;
  //   // },
  //   doneTodos() {
  //     return this.filterTodos === ""
  //       ? this.todos
  //       : this.todos.filter((item) => item.done === this.filterTodos);
  //   },
  //   undoneCount() {
  //     const doneTodos = this.todos.filter((item) => item.done === false);
  //     return doneTodos.length;
  //   },

  //   allFilter() {
  //     return this.selectedFilter == "all" ? "active" : null;
  //   },
  //   doneFilter() {
  //     return this.selectedFilter == "done" ? "active" : null;
  //   },
  //   undoneFilter() {
  //     return this.selectedFilter == "undone" ? "active" : null;
  //   },
  // },
  // methods: {
  //   setFilter(filterTab) {
  //     this.selectedFilter = filterTab;
  //     if (filterTab === "done") {
  //       this.filterTodos = true;
  //       return;
  //     }
  //     if (filterTab === "undone") {
  //       this.filterTodos = false;
  //       return;
  //     }
  //     if (filterTab === "all") {
  //       this.filterTodos = "";
  //       return;
  //     }
  //   },
  //   editTodo(todoId) {
  //     // this.editValue = !this.editValue;
  //     const idx = this.todos.findIndex((idx) => idx.id === todoId);
  //     this.todos[idx].isEdit = true;
  //     this.newValue = this.todos[idx].content;
  //   },
  //   sendEditTodo(todoId) {
  //     const idx = this.todos.findIndex((idx) => idx.id === todoId);
  //     if (this.newValue != "" && this.newValue != this.todos[idx].content) {
  //       this.todos[idx].content = this.newValue;
  //       this.todos[idx].done = false;
  //     }
  //     console.log(this.todos[idx].content);
  //     this.todos[idx].isEdit = false;
  //     this.newValue = "";
  //     this.saveData();
  //   },
  //   removeTodo(todoId) {
  //     const idx = this.todos.findIndex((idx) => idx.id === todoId);
  //     this.todos.splice(idx, 1);
  //     this.saveData();
  //   },
  //   newDone(todoId) {
  //     const idx = this.todos.findIndex((idx) => idx.id === todoId);
  //     this.todos[idx].done = !this.todos[idx].done;
  //     this.saveData();
  //   },
  // },
};
</script>
<style lang="scss" scope>
.list_title {
  border-bottom: 2px solid;
  border-color: rgb(92, 168, 255);
  h3 {
    margin: 0;
  }

  .filter_style {
    text-align: end;
    color: rgb(52, 52, 148);
    .active {
      background-color: rgb(208, 230, 255);
      // color: white;
    }
    button {
      padding: 0.4rem;
      margin: 1px;
      font-weight: 700;
      border-radius: 5px;
      background-color: rgba(92, 168, 255, 0.671);
      border: 2px solid rgb(92, 168, 255);
      border-bottom: none;
    }
  }
}

ul {
  padding: 0;
  .donestyle {
    border-color: rgb(105, 105, 105);
    background-color: unset;
    span {
      color: rgba(195, 223, 255, 0.274);
      text-decoration: line-through;
    }
    button {
      color: rgb(71, 127, 190);
      background-color: rgba(162, 201, 247, 0.719);
    }
  }
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 2px solid rgb(92, 168, 255);
    border-radius: 5px;
    margin: 1rem 0;
    padding: 0.8rem;
    background-color: rgba(122, 184, 255, 0.445);
    img {
      position: absolute;
      opacity: 0.2;
    }
    span {
      padding-left: 30px;
      user-select: none;
    }
    button {
      padding: 0.4rem;
      margin: 0 2px;
      font-weight: 700;
      border-radius: 5px;
      background-color: rgb(208, 230, 255);
      border: none;
      color: rgb(57, 57, 216);
    }
  }
}
</style>