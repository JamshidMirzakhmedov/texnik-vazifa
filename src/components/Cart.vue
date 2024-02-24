<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="modal">
      <h2>Shopping Cart</h2>
      <ul>
        <li v-for="(item, index) in cartItems" :key="index">
          <img :src="item.thumbnail" alt="" />
          {{ item.title }} - ${{ item.price }}
          <button @click="removeFromCart(index)">Remove</button>
        </li>
      </ul>
      <p>Total: ${{ calculateTotal() }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["product"],
  data() {
    return {
      cartItems: [],
    };
  },
  mounted() {
    this.cartItems.push(this.product);
  },
  methods: {
    addToCart(item) {
      this.cartItems.push(item);
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1);
    },
    calculateTotal() {
      return this.cartItems.reduce((total, item) => total + item.price, 0);
    },
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>

<style scoped>
.backdrop {
  z-index: 1000;
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}
.modal {
  width: 600px;
  padding: 20px;
  margin: 100px auto;
  background: white;
  border-radius: 10px;
}
li {
  margin: 50px 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
li img {
  width: 70px;
}
@media (max-width: 600px) {
  .modal {
    width: 90%;
    padding: 10px;
  }
  li {
    margin: 10px 0;
    justify-content: space-between;
  }
}
</style>
