<template>
    <section v-show="isPageOpen" class="user-page-modal" @click.self="$emit('close-page')">
      <div class="user-page-content container">
        <div class="user-page-content-info">
          <h2 class="user-page-info-title title">Мои покупки</h2>
        </div>
  
        <div class="user-page-content-body">
          <div class="user-page-body-item" v-if="orders.length > 0">
            <div v-for="item in orders" :key="item.id" class="user-page-item">
              <img :src="item.image" :alt="item.title" class="user-page-item-img">
              <div class="user-page-item-info">
                <span class="product-info-name">{{ item.title }} ({{ item.quantity }})</span>
                <span class="product-info-text">{{ item.text }}</span>
                <p class="product-price-info">{{ item.price }} ₽</p>
              </div>
            </div>
          </div>

        </div>
        <div class="user-page-sapling" v-if="orders.length === 0">
            <div class="user-page-sapling-info">
                <img src="/src/img/UserPage.jpg" alt="" class="user-page-sapling-img">
                <p class="user-page-sapling-name">У Вас пока не было покупок</p>
                <button class="user-page-sapling-btn" @click="$emit('close-page')">Вернуться</button>
            </div>
          </div>
      </div>
    </section>
</template>
  
<script>
export default {
  props: {
    isPageOpen: Boolean,
    orders: {
      type: Array,
      default: () => [] // Если orders не переданы, будет пустой массив
    }
  },
  watch: {
    isPageOpen(newVal) {
      if (newVal) {
        setTimeout(() => {
          this.$emit('refresh-orders', []); // Передаём пустой массив, чтобы очистить заказы
        }, 180000); // 180000 миллисекунд = 3 минуты
      }
    }
  },
  methods: {
    submitOrder() {
      this.$emit('submit-order', this.orders); // Отправляем заказ в UserPage
      this.$emit('open-page'); // Открываем страницу покупок
    }
  }
};
</script>
<style>
.product-price-info {
    font-family: "Roboto", sans-serif;
    font-weight: 400;
    font-size: 36px;
    color: #000;
}
</style>