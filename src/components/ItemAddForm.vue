<template>
  <form>
    <h4>Добавить таск</h4>
    <input type="text" v-model="label" placeholder="Таск" />
    <div class="category-selector">
      <select
        class="custom-select"
        v-model="selectedCatId"
        @change="$emit('on-select', selectedCatId)"
      >
        <option
          v-for="category in categories"
          :value="category.id"
          :key="category.id"
          >{{ category.name }}</option
        >
        {{
          selectedCatId
        }}
      </select>
    </div>

    <button type="button" @click.prevent="onAdd">
      Add
      <span class="material-icons">add_task</span>
    </button>
  </form>
</template>

<script>
export default {
  name: "AddItem",
  components: {
    // CatSelector,
  },
  data() {
    return {
      label: "",
      selectedCatId: 0,
      isDone: false,
    };
  },
  props: ["categories"],
  methods: {
    onAdd() {
      if (this.label) {
        this.$emit("on-task", this.label, this.selectedCatId, this.isDone);
      }
    },
    onSelectCategory() {
      this.selectedCatId = this.categories.id;
    },
    clearAddForm() {
      this.label = " ";
    },
  },
};
</script>
