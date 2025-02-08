<template>
    <section class="product">
        <div class="product__content container">
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
                    :image="product.image"
                    :favourite="product.favourite"
                    :title="product.title"
                    :text="product.text"
                    :price="product.price"
                    @add-to-cart="addToCart(product)"
                />
            </div>
            <Basket :cart="cart" @update-cart="updateCart" />
        </div>
    </section>
</template>
<script>
import ProductsList from './ProductsList.vue'; // Импорт компонента списка продуктов
import Basket from './Basket.vue'; // Импорт компонента корзины

export default {
    components: {
        ProductsList, // Регистрация компонента списка продуктов
        Basket // Регистрация компонента корзины
    },
    data() {
        return {
            searchQuery: "", // Строка для поиска товаров
            products: [ // Список товаров с изображением, названием, описанием и ценой
                { id: 1, image: "/src/img/products-1.png", favourite: "/src/img/favourite-products-1.png", title: "Blacksi", text: "Костюм спортивный", price: 3595 },
                { id: 2, image: "/src/img/products-2.png", favourite: "/src/img/favourite-products-1.png", title: "Fashion.Love.Story", text: "Платье", price: 3500 },
                { id: 3, image: "/src/img/products-3.png", favourite: "/src/img/favourite-products-1.png", title: "UNIQLO", text: "Водолазка", price: 2999 },
                { id: 4, image: "/src/img/products-4.png", favourite: "/src/img/favourite-products-1.png", title: "Mark Formelle", text: "Костюм домашний", price: 1699 },
                { id: 5, image: "/src/img/products-5.png", favourite: "/src/img/favourite-products-1.png", title: "Vittoria Vicci", text: "Пуловер", price: 3595 },
                { id: 6, image: "/src/img/products-6.png", favourite: "/src/img/favourite-products-1.png", title: "O'stin", text: "Платье", price: 3799 }
            ],
            cart: [] // Массив товаров, добавленных в корзину
        };
    },
    computed: {
        filteredProducts() { // Фильтрация товаров по поисковому запросу
            if (!this.searchQuery) {
                return this.products;
            }
            const query = this.searchQuery.toLowerCase();
            return this.products.filter(product =>
                product.title.toLowerCase().includes(query) || 
                product.text.toLowerCase().includes(query)
            );
        }
    },
    methods: {
        searchProducts() { // Метод поиска товаров (в текущем виде только логирует запрос)
            console.log("Поиск: ", this.searchQuery);
        },
        removeFromCart(productId) { // Удаление товара из корзины
            const productIndex = this.cart.findIndex(item => item.id === productId);
            if (productIndex !== -1) {
                if (this.cart[productIndex].quantity > 1) {
                    this.cart[productIndex].quantity--; // Уменьшение количества товара
                } else {
                    this.cart.splice(productIndex, 1); // Удаление товара из корзины, если его количество 1
                }
            }
        },
        updateCart(updatedCart) { // Обновление состояния корзины (например, после изменения количества в компоненте Basket)
            this.cart = updatedCart;
        }
    }
};
</script>







