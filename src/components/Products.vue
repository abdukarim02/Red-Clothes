<template>
  <section class="product">
      <div class="product__content container">
          <div class="product__content-info">
              <h2 class="product__info-title title">Все товары</h2>
              <form class="product__info-form" @submit.prevent>
                  <button type="submit" class="product__form-btn">
                      <img src="/src/img/search.png" alt="Поиск">
                  </button>
                  <input 
                      type="search" 
                      class="product__form-input" 
                      placeholder="Поиск..." 
                      v-model="searchQuery"
                  >
              </form>
          </div>
          <div class="product__content-wrap">
              <ProductCard 
                  v-for="item in filteredProducts" 
                  :key="item.id" 
                  v-bind="item" 
                  @add-to-cart="addToCart" 
              />

              <Basket
                  :isCartOpen="isCartOpen"
                  :cart="cart"
                  :totalPrice="totalPrice"
                  @close-cart="toggleCart"
                  @update-cart="updateCart"
                  @submit-order="submitOrder"  
              />
          </div>
      </div>
  </section>
</template>

<script>
import { ref, computed, watch } from "vue";
import ProductCard from "./ProductsList.vue";
import Basket from "./Basket.vue";

export default {
  components: {
    ProductCard,
    Basket,
  },
  setup() {
    const searchQuery = ref("");
    const isCartOpen = ref(false);
    const cart = ref([]);

    const products = ref([
      { id: 1, image: "/src/img/products-1.png", favourite: "/src/img/favourite-products-1.png", title: "Blacksi", text: "Костюм спортивный", price: 3595 },
      { id: 2, image: "/src/img/products-2.png", favourite: "/src/img/favourite-products-1.png", title: "Fashion.Love.Story", text: "Платье", price: 3500 },
      { id: 3, image: "/src/img/products-3.png", favourite: "/src/img/favourite-products-1.png", title: "UNIQLO", text: "Водолазка", price: 2999 },
      { id: 4, image: "/src/img/products-4.png", favourite: "/src/img/favourite-products-1.png", title: "Mark Formelle", text: "Костюм домашний", price: 1699 },
      { id: 5, image: "/src/img/products-5.png", favourite: "/src/img/favourite-products-1.png", title: "Vittoria Vicci", text: "Пуловер", price: 3595 },
      { id: 6, image: "/src/img/products-6.png", favourite: "/src/img/favourite-products-1.png", title: "O'stin", text: "Платье", price: 3799 },
    ]);

    const filteredProducts = computed(() => {
      if (!searchQuery.value) return products.value;
      const query = searchQuery.value.toLowerCase();
      return products.value.filter(
        (product) =>
          product.title.toLowerCase().includes(query) ||
          product.text.toLowerCase().includes(query)
      );
    });

    const addToCart = (product) => {
      const existingItem = cart.value.find((item) => item.id === product.id);

      if (existingItem) {
        existingItem.quantity++;
      } else {
        cart.value.push({ ...product, quantity: 1 });
      }
    };

    const removeFromCart = (productId) => {
      cart.value = cart.value.filter((item) => {
        if (item.id === productId) {
          item.quantity--;
          return item.quantity > 0;
        }
        return true;
      });
    };

    const updateCart = (updatedCart) => {
      cart.value = updatedCart;
    };

    const submitOrder = () => {
      if (cart.value.length === 0) return;
      console.log("Заказ оформлен:", cart.value);
      cart.value = [];
    };

    const totalPrice = ref(0);
    watch(cart, () => {
      totalPrice.value = cart.value.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }, { deep: true });

    const toggleCart = () => {
      isCartOpen.value = !isCartOpen.value;
    };

    return {
      searchQuery,
      isCartOpen,
      cart,
      products,
      filteredProducts,
      addToCart,
      removeFromCart,
      updateCart,
      submitOrder,
      totalPrice,
      toggleCart,
    };
  },
};
</script>













