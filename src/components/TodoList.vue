<template>
  <ul>
    <li
      v-bind:class="{ donetask: item.isDone }"
      v-for="item in todos"
      :key="item"
    >
      <p @click="onDoneTask(item)" v-bind:class="{ done: item.isDone }">
        {{ item.label }}
      </p>
      <div>
        <input
          class="edit-field"
          type="hidden"
          placeholder="Заменить"
          value
          v-model="editedTask"
        />
        <button
          @click="onDoneEditing(item)"
          class="material-icons edit-done"
          style="display: none"
        >
          done
        </button>
        <button
          @click="onCancelEditing(item)"
          class="material-icons edit-cancel"
          style="display: none"
        >
          cancel
        </button>
        <button @click="onEditTask(item)" class="material-icons">edit</button>
        <button @click="onDeleteTask(item)" class="material-icons">
          delete
        </button>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  name: "TodoList",
  props: ["todos", "categories"],
  data() {
    return {
      label: "",
    };
  },
  methods: {
    onDeleteTask: function(item) {
      this.$emit("on-delete", item);
    },
    onEditTask: function(item) {
      this.$emit("on-edit", item);
    },
    onDoneEditing: function(item) {
      this.$emit("on-edit-done", item, this.editedTask);
    },
    onCancelEditing: function(item) {
      this.$emit("on-edit-cancel", item, this.editedTask);
    },
    onDoneTask(item) {
      this.$emit("on-done", item, this.isDone);
    },
  },
};
</script>

<style scoped></style>
