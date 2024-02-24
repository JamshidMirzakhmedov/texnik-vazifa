<template>
  <Filter :products="products" @categorySelected="handleCategorySelected" />
  <div class="product-list">
    <ul class="product-grid" v-if="paginatedProducts.length">
      <li
        class="product-item"
        v-for="product in paginatedProducts"
        :key="product.id"
      >
        <router-link
          :to="{ name: 'ProductDetails', params: { id: product.id } }"
        >
          <div class="product-img">
            <img :src="product.thumbnail" :alt="product.title" />
          </div>
          <p class="product-title">{{ product.title }}</p>
          <span class="product-price">${{ product.price }}</span>
          <button>Buy</button>
        </router-link>
      </li>
    </ul>

    <h2 v-else>Loading...</h2>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">
        Next
      </button>
    </div>
  </div>
</template>

<script>
import Filter from "@/components/Filter.vue";
import ProductDetails from "./ProductDetails.vue";
export default {
  components: { Filter, ProductDetails },
  data() {
    return {
      products: [],
      currentPage: 1,
      itemsPerPage: 30,
      selectedCategory: "all",
      searchQuery: "",
    };
  },

  computed: {
    paginatedProducts() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.products.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.products.length / this.itemsPerPage);
    },
  },

  mounted() {
    this.fetchProducts();
  },

  methods: {
    fetchProducts() {
      let apiUrl =
        this.selectedCategory && this.selectedCategory !== "all"
          ? `https://dummyjson.com/products/category/${this.selectedCategory}`
          : `https://dummyjson.com/products?limit=100`;

      fetch(apiUrl)
        .then((res) => res.json())
        .then((data) => (this.products = data.products));
    },

    handleCategorySelected({ category }) {
      this.selectedCategory = category;

      this.fetchProducts();
      this.currentPage = 1;
    },

    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
  },
};
</script>

<style>
.product-list {
  padding: 2% 5%;
  color: black;
}
.product-grid {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  padding: 0;
  margin: 0;
  gap: 30px;
  z-index: 1 !important;
}
.product-item {
  position: relative;
  flex: 0 0 20%;
  box-sizing: border-box;
  padding: 10px 10px 20px 10px;
  margin-bottom: 20px;
  background-color: #fff;
}
.product-item a {
  text-decoration: none;
  color: inherit;
}
.product-item:hover {
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}
.product-img {
  overflow: hidden;
  border-radius: 8px;
}

.product-img img {
  max-width: 240px;
  min-width: 240px;
  min-height: 250px;
  max-height: 250px;
  border-radius: 8px;
}

.product-title {
  font-weight: bold;
}

.product-price {
  color: #007bff;
}

.product-item button {
  position: absolute;
  right: 5px;
  padding: 5px 30px;
  background: inherit;
  outline: none;
  border: 1px solid #005c67;
}

.pagination {
  text-align: center;
}
button {
  margin: 10px;
  padding: 5px 30px;
  background: inherit;
  outline: none;
  border: 1px solid #005c67;
}
@media (max-width: 768px) {
  .product-list {
    padding: 0;
  }
  .product-grid {
    justify-content: center;
    padding: 0;
    margin: 0;
  }
  .product-item {
    flex: 0 0 40%;
  }
  .product-img img {
    max-width: 100%;
    min-width: 100%;
    min-height: 200px;
    max-height: 200px;
  }
}
</style>
