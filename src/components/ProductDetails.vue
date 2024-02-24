<template>
  <Cart :product="product" v-if="showCart" @close="handleBuy" />
  <div class="product-container" v-if="product">
    <div class="action">
      <span class="product-price">${{ product.price }}</span>
      <button class="buy-button" @click="handleBuy">Buy</button>
    </div>

    <swiper
      :modules="modules"
      :slides-per-view="3"
      :space-between="30"
      @swiper="onSwiper"
      @slideChange="onSlideChange"
    >
      <swiper-slide v-for="(image, index) in product.images" :key="index">
        <div class="product-details">
          <div class="product-img">
            <img :src="image" :alt="product.title" />
          </div>
        </div>
      </swiper-slide>
    </swiper>
    <div class="product-info">
      <h2 class="product-title">{{ product.title }}</h2>
      <p class="product-description">{{ product.description }}</p>
    </div>
  </div>
  <h1 v-else>Loading...</h1>
</template>

<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import Cart from "./Cart.vue";

export default {
  name: "ProductDetails",
  components: {
    Swiper,
    SwiperSlide,
    Cart,
  },
  props: ["id"],
  data() {
    return {
      product: {},
      showCart: false,
    };
  },
  mounted() {
    this.fetchProduct();
  },
  methods: {
    fetchProduct() {
      fetch(`https://dummyjson.com/products/${this.id}`)
        .then((res) => res.json())
        .then((data) => {
          this.product = data;
        })
        .catch((err) => console.log(err));
    },
    handleBuy() {
      this.showCart = !this.showCart;
    },
  },
  setup() {
    const onSwiper = (swiper) => {
      // console.log(swiper);
    };
    const onSlideChange = () => {
      // console.log("slide change");
    };
    return {
      onSwiper,
      onSlideChange,
    };
  },
};
</script>

<style scoped>
.product-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  padding-top: 8rem;
  position: relative;
}
.product-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.product-details {
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.product-img img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.product-info {
  margin-top: 20px;
}

h2 {
  color: #333;
  font-size: 20px;
}

.product-description {
  color: #666;
  margin-top: 10px;
}

.product-price {
  display: flex;
  align-items: center;
  margin-top: 10px;
}

.product-price {
  font-weight: bolder;
  color: #333;
  padding: 10px 20px;
  font-size: 20px;
  margin-right: 10px;
}
.action {
  display: flex;
  position: absolute;
  right: 0;
  top: 30px;
}
.buy-button {
  background-color: #4caf50;
  color: white;
  padding: 10px 40px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.buy-button:hover {
  background-color: #45a049;
}
</style>
