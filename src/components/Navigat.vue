<template>
  <header class="header">
    <div class="header__content container">
      <a href="#" class="header__content-logo">
        <img src="/src/img/logo.svg" alt="Логотип">
      </a>
      <div class="header__content-social">
        <div class="header__social-logo" @click="toggleCart">
          <img src="/src/img/basket.svg" alt="Корзина" class="header__logo-img">
          <span>{{ totalPrice }} ₽</span>
        </div>
        <div class="header__social-logo">
          <img src="/src/img/favourite.svg" alt="Избранное" class="header__logo-img">
        </div>
        <div class="header__social-logo">
          <img src="/src/img/user.svg" alt="Профиль" class="header__logo-img">
        </div>
      </div>
    </div>
  </header>

  <!-- Компонент корзины -->
  <Basket :isCartOpen="isCartOpen" :cart="cart" @update-cart="updateCart" @close-cart="toggleCart" />
</template>

<script>
import Basket from './Basket.vue';
import ProductsList from './ProductsList.vue';
import Products from './Products.vue';

export default {
  components: {
    Basket,
    ProductsList,
    Products
  },
  data() {
    return {
      isCartOpen: false,
      cart: []
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }
  },
  methods: {
    toggleCart() {
      this.isCartOpen = !this.isCartOpen;
    },
    updateCart(updatedCart) {  // Добавляем обработчик события update-cart
      this.cart = updatedCart;
    }
  }
};
</script>


