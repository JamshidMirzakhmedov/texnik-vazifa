<template>
  <div class="filter">
    <select v-model="selectedCategory" @change="emitCategory">
      <option value="all" selected>All</option>
      <option v-for="category in categories" :value="category">
        {{ category }}
      </option>
    </select>
    <p>Total products: {{ this.products.length }}</p>
  </div>
</template>

<script>
export default {
  props: ["products"],
  data() {
    return {
      categories: [],
      selectedCategory: null,
    };
  },
  mounted() {
    fetch("https://dummyjson.com/products/categories")
      .then((res) => res.json())
      .then((data) => (this.categories = data));
  },
  methods: {
    emitCategory() {
      this.$emit("categorySelected", {
        category: this.selectedCategory,
      });
    },
  },
};
</script>

<style>
.filter {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  padding: 10px 50px;
}

.filter input[type="text"],
.filter select {
  font-size: 16px;
  padding: 5px;
}
</style>
