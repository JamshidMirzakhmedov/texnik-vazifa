<script>
import { RouterLink, RouterView } from "vue-router";
import Cart from "./components/Cart.vue";

export default {
  components: {
    Cart,
    RouterView,
    RouterLink,
  },
  data() {
    return {
      showCart: false,
    };
  },
  methods: {
    toggleCart() {
      this.showCart = !this.showCart;
    },
  },
};
</script>

<template>
  <header>
    <RouterLink to="/" class="logo">
      <img src="./assets/logo.png" alt="logo" />
    </RouterLink>
    <nav>
      <a href="#">News</a>
      <a href="#">Categories</a>
      <a href="#">Sales</a>
      <a href="#">Contacts</a>
      <a href="#">About us</a>
    </nav>
    <div class="cart" @click="toggleCart">
      <img src="./assets/cart.jpeg" alt="shopping cart" />
    </div>
  </header>

  <RouterView />
  <Cart v-if="showCart" @close="toggleCart" />
</template>

<style>
header {
  height: 117px;
  background-color: #ffff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 2rem;
}

.logo img {
  position: absolute;
  width: 138px;
  height: 105px;
  top: 6px;
  left: 72px;
}

header nav a {
  text-decoration: none;
  font-size: 16px;
  font-weight: 400;
  margin-right: 50px;
}
.cart {
  cursor: pointer;
}
.cart img {
  width: 37.3px;
}
@media (max-width: 768px) {
  header {
    padding: 0 1rem;
  }
  .logo img {
    width: 70px;
    height: 76px;
    top: 6px;
    left: 72px;
  }
  header nav a {
    display: none;
  }
  .cart img {
    width: 30px;
  }
}
</style>
