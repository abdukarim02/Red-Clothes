<template>
  <section v-show="isCartOpen" class="cart-modal" @click.self="$emit('close-cart')">
    <div class="cart-content">
      <div class="cart-top">
        <button @click="$emit('close-cart')" class="close-btn">
          <svg width="56" height="56" viewBox="0 0 56 56" fill="none">
            <rect x="2" y="2" width="52" height="52" rx="26" stroke="#750000" stroke-width="4" />
            <path fill-rule="evenodd" clip-rule="evenodd" d="M21.6 39.4c-.8.8-2.1.8-2.8 0L8.6 29.4a2 2 0 010-2.8L18.8 16.6c.8-.8 2.1-.8 2.8 0s.8 2.1 0 2.8L14.9 26H46a2 2 0 010 4H14.9l6.7 6.6c.8.8.8 2.1 0 2.8z" fill="#750000"/>
          </svg>
        </button>
        <h2 class="title">Корзина</h2>
      </div>

      <div class="cart-body" v-if="cart.length > 0 && !showShipping">
        <div v-for="item in cart" :key="item.id" class="cart-item">
          <img :src="item.image" :alt="item.title" class="cart-item-img">
          <div class="cart-item-info">
            <span class="product__info-name">{{ item.title }} ({{ item.quantity }})</span>
            <span class="product__info-text">{{ item.text }}</span>
            <p class="product__price-info">{{ item.price }} ₽</p>
          </div>
          <button @click="removeFromCart(item.id)" class="product__footer-btn">X</button>
        </div>
      </div>

      <div class="cart-sapling" v-if="cart.length === 0 && !showShipping">
        <div class="cart-sapling-info">
          <h5 class="cart-sapling-name">Ваша корзина пуста</h5>
          <img src="@/img/cart-sapling-1.png" alt="" class="cart-sapling-img">
          <p class="cart-sapling-text">Добавьте хотя бы один товар, чтобы сделать заказ</p>
          <button class="cart-sapling-btn" @click="$emit('close-cart')">Вернуться</button>
        </div>
      </div>

      <div class="cart-shipping" v-if="showShipping">
        <div class="cart-sapling-info">
          <h5 class="cart-sapling-name">Заказ оформлен!</h5>
          <img src="@/img/cart-sapling-2.png" alt="" class="cart-sapling-img">
          <p class="cart-sapling-text">Спасибо за ваш заказ!</p>
          <button class="cart-sapling-btn" @click="closeShipping">Ок</button>
        </div>
      </div>

      <div class="cart-footer" v-if="cart.length > 0 && !showShipping">
        <p class="product__info-title title">К оплате: <span>{{ totalPrice }} ₽</span></p>
        <button class="cart-footer-btn" @click="submitOrder">Заказать</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    isCartOpen: Boolean, // Флаг, открыта ли корзина
    cart: Array // Массив товаров в корзине
  },
  data() {
    return {
      showShipping: false // Флаг отображения сообщения об успешном заказе
    };
  },
  computed: {
    totalPrice() {
      // Вычисляем общую стоимость товаров в корзине
      return this.cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }
  },
  methods: {
    removeFromCart(id) {
      // Уменьшаем количество товара или удаляем его, если количество становится 0
      let updatedCart = this.cart.map(item => {
        if (item.id === id) {
          return { ...item, quantity: item.quantity - 1 }; // Уменьшаем количество
        }
        return item;
      }).filter(item => item.quantity > 0); // Убираем товар, если quantity <= 0

      this.$emit('cart-modal', updatedCart); // Передаем обновленный массив корзины в родительский компонент
    },

    submitOrder() {
      // Проверяем, есть ли товары в корзине
      if (this.cart.length === 0) {
        console.warn('Корзина пуста, заказ невозможен!');
        return;
      }

      const orderCopy = JSON.parse(JSON.stringify(this.cart)); // Создаем копию заказа, чтобы избежать мутаций

      this.$emit('submit-order', orderCopy); // Отправляем заказ в родительский компонент
      this.showShipping = true; // Показываем сообщение о заказе

      // Закрываем сообщение через 3 секунды
      setTimeout(() => {
        this.closeShipping();
      }, 3000);
      
      console.log(`✅ Заказ оформлен:`, orderCopy);
    },

    closeShipping() {
      this.$emit('update-cart', []); // Очищаем корзину после оформления заказа
      this.showShipping = false; // Скрываем сообщение о заказе
      this.$emit('close-cart'); // Закрываем корзину
    }
  }
};
</script>


<style>
.cart-modal {
    width: 100%;
    height: 100vh;
    background: rgba(0, 0, 0, 0.4);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 30;
}
.cart-content {
    position: relative;
    width: 40%;
    height: 100%;
    padding: 40px 50px;
    background: #fff;
    margin-left: auto;
    box-shadow: -4px 0 4px 0 rgba(0, 0, 0, 0.4);

}
.cart-top {
    display: flex;
    align-items: center;
    gap: 20px;
}
.cart-body {
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 30px;
    overflow-y: auto;
    margin: 50px 0;
}
.cart-item {
    padding: 25px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border: 1px solid #d9d9d9;
    border-radius: 30px;
}
.cart-item-img {
    border: 1px solid #d9d9d9;
    border-radius: 180px;
    width: 150px;
    height: 150px;
    object-fit: cover;
}
.cart-item-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 128px;
}
.cart-footer {
    position: absolute;
    bottom: 0;
    left: 0;
    border-radius: 30px 30px 0 0;
    width: 100%;
    height: 150px;
    padding: 25px 50px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-shadow: 0 -4px 10px 0 rgba(0, 0, 0, 0.5);
    background: linear-gradient(180deg, #d9d9d9 7.08%, #fff 50.31%);
}
.product__info-title span {
    color: #000;
}
.cart-footer-btn {
    border: 4px solid #750000;
    border-radius: 20px;
    padding: 25px 38px;
    font-weight: 700;
    font-size: 36px;
    color: #750000;
}
.cart-sapling {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  margin-top: 80px;
}
.cart-sapling-info {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.cart-sapling-name {
  font-family: "Roboto", sans-serif;
  font-weight: 700;
  font-size: 24px;
  text-align: center;
  margin-bottom: 25px;
  color: #750000;
}
.cart-sapling-text {
  font-family:"Open Sans", sans-serif;
  width: 353px;
  margin: 10px auto 50px;
  font-weight: 400;
  font-size: 24px;
  text-align: center;
  color: #750000;
}
.cart-sapling-btn {
  border-radius: 20px;
  padding: 25px 38px;
  width: 271px;
  height: 99px;
  font-weight: 700;
  font-size: 36px;
  text-align: center;
  color: #750000;
  border: 4px solid #750000;
}
@media (max-width: 1024px) {
  .cart-content {
    width: 100%;
  }
  .cart-footer {
    height: 240px;
  }
  .user-page-content-body {
    width: 100%;
  }
  .user-page-item {
    width: 80%;
  }
}
@media (max-width: 800px) {
  .user-page-item {
    width: 100%;
  }
}
</style>