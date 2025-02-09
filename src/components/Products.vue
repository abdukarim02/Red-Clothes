<template>
    <section class="product">
        <div class="product__content container" >
            <div class="product__content-info">
                <h2 class="product__info-title title">Все товары</h2>
                <form class="product__info-form" @submit.prevent="searchProducts">
                    <button type="submit" class="product__form-btn">
                        <img src="/src/img/search.png" alt="Поиск">
                    </button>
                    <input 
                        type="search" 
                        class="product__form-input" 
                        placeholder="Поиск..." 
                        v-model="searchQuery"
                        @input="searchProducts"
                    >
                </form>
            </div>
            <div class="product__content-wrap">
                <ProductsList 
                    v-for="product in filteredProducts" 
                    :key="product.id"
                    v-bind="product" 
                    @add-to-cart="addToCart"
                />

            </div>
            <Basket 
                :isCartOpen="isCartOpen" 
                :cart="cart"
                @update-cart="updateCart"
                @close-cart="toggleCart"
            />
        </div>
    </section>
</template>

<script>
import ProductsList from './ProductsList.vue';
import Basket from './Basket.vue';

export default {
    components: {
        ProductsList, // Компонент списка товаров
        Basket // Компонент корзины
    },
    data() {
        return {
            searchQuery: "", // Строка поиска
            isCartOpen: false, // Состояние отображения корзины (открыта/закрыта)
            products: [ // Список товаров
                { id: 1, image: "/src/img/products-1.png", favourite: "/src/img/favourite-products-1.png", title: "Blacksi", text: "Костюм спортивный", price: 3595 },
                { id: 2, image: "/src/img/products-2.png", favourite: "/src/img/favourite-products-1.png", title: "Fashion.Love.Story", text: "Платье", price: 3500 },
                { id: 3, image: "/src/img/products-3.png", favourite: "/src/img/favourite-products-1.png", title: "UNIQLO", text: "Водолазка", price: 2999 },
                { id: 4, image: "/src/img/products-4.png", favourite: "/src/img/favourite-products-1.png", title: "Mark Formelle", text: "Костюм домашний", price: 1699 },
                { id: 5, image: "/src/img/products-5.png", favourite: "/src/img/favourite-products-1.png", title: "Vittoria Vicci", text: "Пуловер", price: 3595 },
                { id: 6, image: "/src/img/products-6.png", favourite: "/src/img/favourite-products-1.png", title: "O'stin", text: "Платье", price: 3799 }
            ],
            cart: [] // Корзина товаров
        };
    },
    computed: {
        filteredProducts() {
            // Фильтруем товары по введённому запросу
            if (!this.searchQuery) {
                return this.products; // Если поисковый запрос пуст, возвращаем весь список
            }
            const query = this.searchQuery.toLowerCase();
            return this.products.filter(product =>
                product.title.toLowerCase().includes(query) || // Поиск по названию
                product.text.toLowerCase().includes(query) // Поиск по описанию
            );
        }
    },
    methods: {
        searchProducts() {
            console.log("Поиск: ", this.searchQuery); // Выводим запрос в консоль
        },
        addToCart(product) {
            // Добавление товара в корзину
            const existingIndex = this.cart.findIndex(item => item.id === product.id);
            if (existingIndex !== -1) {
                // Если товар уже в корзине, увеличиваем его количество
                this.cart = this.cart.map((item, index) =>
                    index === existingIndex ? { ...item, quantity: item.quantity + 1 } : item
                );
            } else {
                // Если товара нет в корзине, добавляем его с quantity = 1
                this.cart = [...this.cart, { ...product, quantity: 1 }];
            }
        },
        removeFromCart(productId) {
            // Удаление товара из корзины или уменьшение его количества
            this.cart = this.cart.map(item => 
                item.id === productId ? { ...item, quantity: item.quantity - 1 } : item
            ).filter(item => item.quantity > 0); // Удаляем товар, если quantity стало 0
        },
        updateCart(updatedCart) {
            this.cart = updatedCart; // Обновляем корзину при изменениях
        },
        toggleCart() {
            this.isCartOpen = !this.isCartOpen; // Открыть/закрыть корзину
        }
    }
};

</script>








