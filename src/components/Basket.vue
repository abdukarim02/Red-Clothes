<template>
    <section v-show="isCartOpen" class="cart-modal" @click.self="$emit('close-cart')">
      <div class="cart-content">
        <div class="cart-top">
            <button @click="$emit('close-cart')" class="close-btn">
                <svg width="56" height="56" viewBox="0 0 56 56" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <rect x="2" y="2" width="52" height="52" rx="26" stroke="#750000" stroke-width="4" />
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M21.6087 39.4014C20.8347 40.1895 19.5685 40.2009 18.7804 39.4269L8.59858 29.4269C8.21569 29.0508 8 28.5367 8 28C8 27.4633 8.21569 26.9492 8.59858 26.5731L18.7804 16.5731C19.5685 15.7991 20.8347 15.8105 21.6087 16.5986C22.3827 17.3866 22.3713 18.6529 21.5832 19.4269L14.8906 26L46 26C47.1046 26 48 26.8954 48 28C48 29.1046 47.1046 30 46 30L14.8906 30L21.5832 36.5731C22.3713 37.3471 22.3827 38.6134 21.6087 39.4014Z" fill="#750000" />
                </svg>
            </button>
            <h2 class="title">Корзина</h2>
        </div>
        
        <div class="cart-body" v-if="cart.length > 0">
          <div v-for="item in cart" :key="item.id" class="cart-item">
            <img :src="item.imag" :alt="item.title" class="cart-item-img">
            <div class="cart-item-info">
              <span class="product__info-name">{{ item.title }} ({{ item.quantity }})</span>
              <span class="product__info-text">{{ item.text }}</span>
              <p class="product__price-info">{{ item.price }} ₽</p>
            </div>
            <button @click="removeFromCart(item.id)" class="product__footer-btn">X</button>
          </div>
        </div>
        <div v-else> Корзина пуста</div>
        
        <div class="cart-footer">
          <p class="product__info-title title">К оплате: <span>{{ totalPrice }} ₽</span></p>
          <button class="cart-footer-btn" @click="submitOrder">Заказать</button>
        </div>
      </div>
    </section>
</template>

<script>
export default {
  props: {
    isCartOpen: Boolean, // Показывать корзину
    cart: Array // Список товаров в корзине
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }
  },
  methods: {
    removeFromCart(id) {
      this.$emit('remove-from-cart', id);
    },
    async submitOrder() {
      if (this.cart.length === 0) {
        alert("Ваша корзина пуста!");
        return;
      }

      try {
        const response = await fetch("https://example.com/api/orders", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ items: this.cart, total: this.totalPrice })
        });

        if (!response.ok) throw new Error("Ошибка при оформлении заказа");

        alert("Заказ успешно отправлен!");
        this.$emit('clear-cart'); // Очистить корзину после успешного заказа
        this.$emit('close-cart'); // Закрыть корзину
      } catch (error) {
        console.error("Ошибка при заказе:", error);
        alert("Ошибка при оформлении заказа. Попробуйте снова.");
      }
    }
  }
};
</script>

<style>
.cart-modal {
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.4);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 10;
}
.cart-content {
    position: relative;
    width: 40%;
    height: 1080px;
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
    height: 740px;
    display: flex;
    flex-direction: column;
    gap: 30px;
    overflow-y: auto;
    margin-top: 50px;
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
</style>

  
