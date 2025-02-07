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
  <Basket :isCartOpen="isCartOpen" @close-cart="toggleCart" :cart="cart"/>
</template>

<script>
import Basket from './Basket.vue';

export default {
  components: {
    Basket
  },
  data() {
    return {
      isCartOpen: false,
      cart: [
        { id: 1, imag: "/src/img/products-1.png", title: "Blacksi", quantity: 1, price: 3595 },
        { id: 2, imag: "/src/img/products-2.png", title: "Nike Air", quantity: 2, price: 7999 }
      ]
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
    }
  }
};
</script>
