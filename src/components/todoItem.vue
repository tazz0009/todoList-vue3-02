<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneEdit" />
      <div
        v-if="!editing"
        @dblclick="editTodo()"
        class="todo-item-label"
        :class="{ completed: completed }"
      >
        {{ title }}
      </div>
      <input
        v-else
        class="todo-item-edit"
        type="text"
        v-model="title"
        @blur="doneEdit"
        @keyup.enter="doneEdit"
        @keyup.esc="cencelEdit"
        v-focus
      />
    </div>
    <div class="remove-item" @click="removeTodo(index)">
      &times;
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-item",
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforEditCache: "",
    };
  },
  directives: {
    focus: {
      // directive definition
      mounted(el) {
        el.focus();
      },
    },
  },
  watch: {
    checkAll() {
      this.completed = this.checkAll ? true : this.todo.completed;
    },
  },
  methods: {
    removeTodo(index) {
      this.$emit("removeTodo", index);
    },
    editTodo() {
      this.beforEditCache = this.title;
      this.editing = true;
    },
    doneEdit() {
      if (this.title.trim() == "") {
        this.title = this.beforEditCache;
        return;
      }
      this.editing = false;
      this.$emit("finishedEdit", {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          completed: this.completed,
          editing: this.editing,
        },
      });
    },
    cencelEdit() {
      this.title = this.beforEditCache;
      this.editing = false;
    },
  },
};
</script>

<style></style>
