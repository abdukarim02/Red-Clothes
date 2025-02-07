<template>
    <div class="product__wrap-flex">
        <div class="product__flex-bg">
            <img :src="imag" alt="product-bg" class="product__bg-img">
            <img :src="favourite" alt="favourite" class="product__bg-favourite">
        </div>
        <div class="product__flex-info">
            <h5 class="product__info-name">{{ title }}</h5>
            <p class="product__info-text">{{ text }}</p>
            <div class="product__info-footer">
                <div class="product__footer-price">
                    <p class="product__price-name">ЦЕНА:</p>
                    <span class="product__price-info">{{ price }}</span>
                </div>
                <button @click="addToCart(product)" class="product__footer-btn">+</button>
            </div>
            
        </div>
    </div>
</template>
<script setup>
defineProps({
    imag: String,
    favourite: String,
    title: String,
    text: String,
    price: Number,
})
</script>