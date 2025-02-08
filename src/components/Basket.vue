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
      
      <div class="cart-body" v-if="cart.length > 0 && !showShipping">
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
      <div class="cart-sapling" v-if="cart.length === 0 && !showShipping">
        <div class="cart-sapling-info">
          <h5 class="cart-sapling-name">Ваша корзина пуста</h5>
          <img src="/src/img/cart-sapling-1.png" alt="" class="cart-sapling-img">
          <p class="cart-sapling-text">Добавьте хотя бы один товар, чтобы сделать заказ</p>
          <button class="cart-sapling-btn" @click="$emit('close-cart')">Вернуться</button>
        </div>
      </div>
      <div class="cart-shipping" v-if="showShipping">
        <div class="cart-sapling-info">
          <h5 class="cart-sapling-name">Заказ оформлен!</h5>
          <img src="/src/img/cart-sapling-2.png" alt="" class="cart-sapling-img">
          <p class="cart-sapling-text">Спасибо за ваш заказ!</p>
          <button class="cart-sapling-btn" @click="$emit('close-cart')">Ок</button>
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
  isCartOpen: Boolean,
  cart: Array
},
data() {
  return {
    showShipping: false
  };
},
computed: {
  totalPrice() {
    return this.cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  }
},
methods: {
  removeFromCart(id) {
    const index = this.cart.findIndex(item => item.id === id);
    if (index !== -1) {
      this.cart.splice(index, 1);
      this.$emit('update-cart', this.cart);
    }
  },
  submitOrder() {
    this.showShipping = true;
  }
}
};
</script>>


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
</style>

  
