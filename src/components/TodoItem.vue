<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" name="status" id="status" v-model="status" @change="donEdit" />
      <div
        v-if="!edited"
        class="todo-item-label"
        @dblclick="edit"
        :class="{chacked : status}"
      >{{title}}</div>
      <input
        v-else
        type="text"
        class="todo-item-edit"
        v-model="title"
        @blur="donEdit"
        @keyup.enter="donEdit"
        @keyup.esc="cancleEdit"
        v-focus
      />
    </div>
    <div class="remove" @click="removeTodo(index)">
      <font-awesome-icon icon="trash" />
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    todos: {
      type: Object,
      require: true
    },
    index: {
      type: Number,
      required: true
    },
    CheckAll: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      id: this.todos.id,
      title: this.todos.title,
      status: this.todos.status,
      edited: this.todos.edit,
      todoCache: ""
    };
  },
  methods: {
    removeTodo(index) {
      this.$emit("removeTodo", index);
    },
    edit() {
      this.todoCache = this.title;
      this.edited = true;
    },
    donEdit() {
      if (this.title.trim().length == 0) {
        this.title = this.todoCache;
        this.edited = false;
      }
      this.edited = false;
      this.$emit("finishEdit", {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          status: this.status,
          edit: this.edited
        }
      });
    },
    cancleEdit() {
      this.edited = false;
      this.title = this.todoCache;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  watch: {
    checkAll() {
      if (this.CheckAll) {
        this.status = true;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
</style>