<template>
  <header class="header">
    <div class="header__content container">
      <a href="#" class="header__content-logo">
        <img src="@/img/logo.svg" alt="Логотип">
      </a>
      <div class="header__content-social">
        <div class="header__social-logo" @click="toggleCart">
          <img src="@/img/basket.svg" alt="Корзина" class="header__logo-img">
          <span>{{ totalPrice }} ₽</span>
        </div>
        <div class="header__social-logo">
          <img src="@/img/favourite.svg" @click="toggleFavoritePage" alt="Избранное" class="header__logo-img">
        </div>
        <div class="header__social-logo" @click="togglePage">
          <img src="@/img/user.svg" alt="Профиль" class="header__logo-img">
        </div>
      </div>
    </div>
  </header>

  <!-- Компонент корзины -->
  <Basket 
    :isCartOpen="isCartOpen" 
    :cart="cart" 
    @update-cart="updateCart" 
    @close-cart="isCartOpen = false" 
    @open-page="isPageOpen = true"
    @submit-order="updateOrders" 
  />
  
  <!-- Компонент страницы пользователя -->
  <UserPage
    :isPageOpen="isPageOpen" 
    :orders="orders" 
    @close-page="isPageOpen = false" 
    @refresh-orders="orders = []" 
  />
</template>

<script>
import Basket from './Basket.vue';
import UserPage from './UserPage.vue';

export default {
  components: {
    Basket, 
    UserPage
  },
  data() {
    return {
      isCartOpen: false,
      isPageOpen: false,
      cart: [
        { id: 1, image: "/src/img/products-1.png", quantity: 1, title: "Blacksi", text: "Костюм спортивный", price: 3595 },
        { id: 2, image: "/src/img/products-2.png", quantity: 2, title: "Blacksi", text: "Костюм спортивный", price: 4595 },
        { id: 3, image: "/src/img/products-3.png", quantity: 3, title: "Blacksi", text: "Костюм спортивный", price: 5595 },
        { id: 4, image: "/src/img/products-4.png", quantity: 1, title: "Blacksi", text: "Костюм спортивный", price: 3595 },
        { id: 5, image: "/src/img/products-5.png", quantity: 2, title: "Blacksi", text: "Костюм спортивный", price: 4595 },
        { id: 6, image: "/src/img/products-6.png", quantity: 3, title: "Blacksi", text: "Костюм спортивный", price: 5595 },
      ],
      orders: []
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((sum, item) => sum + item.price * Number(item.quantity), 0);
    }
  },
  methods: {
    toggleCart() {
      this.isCartOpen = !this.isCartOpen;
    },
    updateCart(updatedCart) {
      this.cart = updatedCart || [];
    },
    togglePage() {
      this.isPageOpen = !this.isPageOpen;
    },
    updateOrders(newOrders) {
      if (Array.isArray(newOrders) && newOrders.length) {
        this.orders.push(...newOrders);
        this.cart = [];
      }
    },
    toggleFavoritePage() {
      console.log("Открытие избранного");
    }
  }
};
</script>









