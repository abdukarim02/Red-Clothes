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
import ProductsList from './ProductsList.vue';
import Products from './Products.vue';

export default {
  components: {
    Basket, // Компонент корзины
    UserPage, // Страница пользователя
    ProductsList, // Список товаров
    Products // Карточка товара
  },
  data() {
    return {
      isCartOpen: false, // Флаг открытия корзины
      isPageOpen: false, // Флаг открытия страницы пользователя
      isFavoriteOpen: false, // Флаг открытия страницы избранного
      cart: [], // Массив товаров в корзине
      favoriteItems: [], // Массив избранных товаров
      orders: [] // ✅ Массив заказов (добавлен)
    };
  },
  computed: {
    totalPrice() {
      // Вычисляем общую сумму товаров в корзине
      return this.cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }
  },
  methods: {
    toggleCart() {
      this.isCartOpen = !this.isCartOpen; // Открываем/закрываем корзину
    },
    updateCart(updatedCart) {
      this.cart = updatedCart; // Обновляем корзину при изменении товаров
    },
    togglePage() {
      this.isPageOpen = !this.isPageOpen; // Открываем/закрываем страницу пользователя
    },
    updateOrders(newOrders) {
      this.orders = newOrders; // Обновляем заказы при их оформлении
    },
    toggleFavoritePage() {
      this.isFavoriteOpen = !this.isFavoriteOpen; // Открываем/закрываем страницу избранного
    }
  }
};
</script>






