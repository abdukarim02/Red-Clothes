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

  <!-- Компонент избранного -->
  <FavoritePage
    v-if="isFavoriteOpen"
    :favorites="favoriteItems" 
    @close-favorite="toggleFavoritePage"
  />
</template>

<script>
import Basket from './Basket.vue';
import UserPage from './UserPage.vue';
import ProductsList from './ProductsList.vue';
import Products from './Products.vue';
import FavoritePage from './FavoritePage.vue';

export default {
  components: {
    Basket,
    UserPage,
    ProductsList,
    Products,
    FavoritePage
  },
  data() {
    return {
      isCartOpen: false,
      isPageOpen: false,
      isFavoriteOpen: false,
      cart: [
        { id: 1, image: "/src/img/products-1.png", title: "Blacksi", quantity: 2, text: "Костюм спортивный", price: 3595 },
        { id: 2, image: "/src/img/products-2.png", title: "Blacksi", quantity: 3, text: "Костюм спортивный", price: 3595 }
      ],
      favoriteItems: []
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((sum, item) => sum + (item.price * item.quantity || 0), 0);
    }
  },
  methods: {
    toggleCart() {
      this.isCartOpen = !this.isCartOpen;
    },
    updateCart(updatedCart) {
      this.cart = updatedCart;
    },
    togglePage() {
      this.isPageOpen = !this.isPageOpen;
    },
    toggleFavoritePage() {
      this.isFavoriteOpen = !this.isFavoriteOpen;
    },
    updateOrders(newOrders) {
      this.orders = newOrders;
    },
    updateFavorites(newFavorites) {
      this.favoriteItems = newFavorites;
    }
  }
};
</script>





