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
                <button @click="addToCart" class="product__footer-btn">+</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

// Определяем входные параметры (props) компонента
const props = defineProps({
    id: Number, // Уникальный идентификатор товара
    image: String, // URL изображения товара
    favourite: String, // URL изображения для избранного (если используется)
    title: String, // Название товара
    text: String, // Описание товара
    price: Number // Цена товара
});

// Определяем события, которые компонент может выбрасывать
const emit = defineEmits(['add-to-cart']);

// Функция для добавления товара в корзину
const addToCart = () => {
    // Проверяем, что все необходимые данные переданы
    if (!props.id || !props.title || !props.text || props.price == null || !props.image) {
        console.warn('Некорректные данные товара:', { 
            id: props.id, 
            title: props.title, 
            text: props.text, 
            price: props.price, 
            image: props.image 
        });
        return; // Прекращаем выполнение, если данные некорректны
    }

    // Вызываем событие 'add-to-cart' и передаём в него объект товара с количеством
    emit('add-to-cart', { ...props, quantity: 1 });

    console.log(`Товар добавлен в корзину: ${props.title} (${props.id}) - ${props.price} ₽`);
};
</script>



