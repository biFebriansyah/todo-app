<template>
  <div>
    <input
      class="todo-input"
      type="text"
      placeholder="what needs to be done"
      v-model="newTodos"
      @keyup.enter="addTodo"
    />
    <transition-group
      name="fade"
      enter-active-class="animated fadeInDown"
      leave-active-class="animated fadeOutDown"
    >
      <div v-for="(todos, index) in todosFilter" :key="todos.id">
        <todo-item
          :todos="todos"
          :index="index"
          :CheckAll="!all"
          @removeTodo="removeTodo"
          @edited="edit"
          @finishEdit="finishing"
        />
      </div>
    </transition-group>

    <div class="extras">
      <any-remaning :anyRemaning="all" />
      <remaning-item :remining="remining" />
    </div>

    <div class="extras">
      <div>
        <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
        <button :class="{active: filter == 'active'}" @click="filter = 'active'">active</button>
        <button :class="{active: filter == 'complete'}" @click="filter = 'complete'">complete</button>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem";
import Remaning from "./Remaning";
import Anyremaning from "./AnyRemaning";

export default {
  name: "todolist",
  components: {
    "todo-item": TodoItem,
    "remaning-item": Remaning,
    "any-remaning": Anyremaning
  },
  data() {
    return {
      newTodos: "",
      todoId: 4,
      todoCache: "",
      filter: "all",
      todo: [
        {
          id: 0,
          title: "Python",
          status: true,
          edit: false
        },
        {
          id: 2,
          title: "Fluter",
          status: false,
          edit: false
        },
        {
          id: 3,
          title: "Golang",
          status: false,
          edit: false
        }
      ]
    };
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  computed: {
    remining() {
      return this.todo.filter(todos => !todos.status).length;
    },
    all() {
      return this.remining != 0;
    },
    todosFilter() {
      if (this.filter == "all") {
        return this.todo;
      }
      if (this.filter == "active") {
        return this.todo.filter(todos => !todos.status);
      }
      if (this.filter == "complete") {
        return this.todo.filter(todos => todos.status);
      }

      return this.todo;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodos.trim().length == 0) {
        alert("log");
        return;
      }

      this.todo.push({
        id: this.todoId,
        title: this.newTodos,
        status: false,
        edit: false
      });

      this.newTodos = "";
      this.todoId++;
    },

    removeTodo(index) {
      this.todo.splice(index, 1);
    },
    edit(todo) {
      this.todoCache = todo.title;
      todo.edit = true;
    },
    donEdit(todo) {
      if (todo.title.trim().length == 0) {
        todo.title = this.todoCache;
        todo.edit = false;
      }
      todo.edit = false;
    },
    cancleEdit(todo) {
      todo.edit = false;
      todo.title = this.todoCache;
    },
    todoChackAll() {
      this.todo.filter(todos => (todos.status = event.target.checked));
    },
    finishing(data) {
      this.todo.splice(data.index, 1, data.todo);
    }
  },
  created() {
    this.$eventBus.$on("check-all", checked => this.todoChackAll(checked));
  },
  beforeDestroy() {
    this.$eventBus.$off("check-all");
  }
};
</script>

<style lang="scss" >
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");

.todo-input {
  width: 93%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-direction: 0.1s;
}

.chacked {
  text-decoration: line-through;
  color: #bbb;
}

.remove {
  cursor: pointer;
  margin-left: 10px;
  color: #bbbb;

  &:hover {
    color: #ed0317;
  }
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
}

.icons {
  width: 30px;
  height: 30px;
  background: #2c3e50;
}

.extras {
  width: 97%;
  display: flex;
  justify-content: space-between;
  font-size: 20px;
  border-top: 1px solid gray;
  padding: 10px;
  color: rgb(92, 92, 92);
}

.active {
  background: green;
  color: white;
}

.fade-enter-active .fade-leave-active {
  transition: opacity 0.2s;
}

.fade-enter .fade-leave-out {
  opacity: 0;
}
</style>