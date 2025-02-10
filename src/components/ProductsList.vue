<template>
    <div class="product__wrap-flex" :data-id="id || ''">
        <div class="product__flex-bg">
            <img :src="image" alt="product-bg" class="product__bg-img">
            <img :src="favourite" alt="favourite" class="product__bg-favourite">
        </div>
        <div class="product__flex-info">
            <h5 class="product__info-name">{{ title }}</h5>
            <p class="product__info-text">{{ text }}</p>
            <div class="product__info-footer">
                <div class="product__footer-price">
                    <p class="product__price-name">ЦЕНА:</p>
                    <span class="product__price-info">{{ price }} ₽</span>
                </div>
                <button 
                    @click="addToCart" 
                    class="product__footer-btn"
                    :disabled="!id || !title || !text || price == null || !image"
                >
                    +
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    id: Number,
    image: String,
    favourite: String,
    title: String,
    text: String,
    price: Number
});

const emit = defineEmits(['add-to-cart']);

const addToCart = () => {
    if (!props.id || !props.title || !props.text || props.price == null || !props.image) {
        console.warn('Некорректные данные товара:', props);
        return;
    }

    emit('add-to-cart', {
        id: props.id,
        image: props.image,
        favourite: props.favourite,
        title: props.title,
        text: props.text,
        price: props.price,
        quantity: 1
    });

    console.log(`Товар добавлен: ${props.title} (${props.id}) - ${props.price} ₽`);
};
</script>




