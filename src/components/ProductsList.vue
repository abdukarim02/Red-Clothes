<template>
    <div class="product__wrap-flex" :data-id="id">
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
                <!-- Кнопка добавления в корзину -->
                <button @click="addToCart" class="product__footer-btn">+</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

// Принимаем данные о товаре
const props = defineProps({
    id: Number, // ID товара
    image: String,
    favourite: String,
    title: String,
    text: String,
    price: Number
});

// Определяем событие для передачи в родительский компонент
const emit = defineEmits(['add-to-cart']);

// Метод для добавления товара в корзину
const addToCart = () => {
    if (!props.id || !props.title || !props.text || !props.price || !props.image) {
        console.warn('Некорректные данные товара:', props);
        return;
    }

    emit('add-to-cart', {
        id: props.id,
        title: props.title,
        text: props.text,
        price: props.price,
        image: props.image,
        quantity: 1
    });

    console.log(`Товар добавлен в корзину: ${props.title} ${props.text} (ID: ${props.id} ) ${props.price}`);
};

</script>


