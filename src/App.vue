<script setup>
import { ref } from "vue";
import Navigat from "./components/Navigat.vue";
import Intro from "./components/Intro.vue";
import Products from "./components/Products.vue";
import QuickLinks from "./components/QuickLinks.vue";
import Basket from "./components/Basket.vue";

const isCartOpen = ref(false);
const cart = ref([]);

// Добавление товара в корзину
const addToCart = (product) => {
  const existingItem = cart.value.find((item) => item.id === product.id);
  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    cart.value.push({ ...product, quantity: 1 });
  }
};

// Обновление корзины
const updateCart = (updatedCart) => {
  cart.value = updatedCart;
};

// Открытие/закрытие корзины
const toggleCart = () => {
  isCartOpen.value = !isCartOpen.value;
};

</script>

<template>
  <Navigat />
  <Intro />
  <Products 
    :cart="cart" 
    :isCartOpen="isCartOpen"
    @add-to-cart="addToCart" 
    @update-cart="updateCart"
    @toggle-cart="toggleCart"
  />
  <Basket 
    :cart="cart" 
    :isCartOpen="isCartOpen"
    @update-cart="updateCart"
    @close-cart="toggleCart"
  />
  <QuickLinks />
</template>


