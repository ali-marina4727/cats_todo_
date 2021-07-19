<template>
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
</template>

<script>
export default {
  name: "CatSelector",
  props: ["categories", "categoryId"],
  data() {
    return {
      selectedCatId: 0,
    };
  },
  watch: {
    categoryId() {
      this.selectedCatId = this.categoryId;
    },
  },
};
</script>
