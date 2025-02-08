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
            <!-- <Basket 
                :isCartOpen="isCartOpen"
                :cart="cart"
                @update-cart="updateCart"
                @close-cart="isCartOpen = false"
            /> -->
        </div>
    </section>
</template>

<script>
import ProductsList from './ProductsList.vue';
import Basket from './Basket.vue';

export default {
    components: {
        ProductsList,
        Basket
    },
    data() {
        return {
            searchQuery: "",
            isCartOpen: false, // Добавлено состояние корзины
            products: [
                { id: 1, image: "/src/img/products-1.png", favourite: "/src/img/favourite-products-1.png", title: "Blacksi", text: "Костюм спортивный", price: 3595 },
                { id: 2, image: "/src/img/products-2.png", favourite: "/src/img/favourite-products-1.png", title: "Fashion.Love.Story", text: "Платье", price: 3500 },
                { id: 3, image: "/src/img/products-3.png", favourite: "/src/img/favourite-products-1.png", title: "UNIQLO", text: "Водолазка", price: 2999 },
                { id: 4, image: "/src/img/products-4.png", favourite: "/src/img/favourite-products-1.png", title: "Mark Formelle", text: "Костюм домашний", price: 1699 },
                { id: 5, image: "/src/img/products-5.png", favourite: "/src/img/favourite-products-1.png", title: "Vittoria Vicci", text: "Пуловер", price: 3595 },
                { id: 6, image: "/src/img/products-6.png", favourite: "/src/img/favourite-products-1.png", title: "O'stin", text: "Платье", price: 3799 }
            ],
            cart: []
        };
    },
    computed: {
        filteredProducts() {
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
        searchProducts() {
            console.log("Поиск: ", this.searchQuery);
        },
        addToCart(product) {
        const existingIndex = this.cart.findIndex(item => item.id === product.id);
        if (existingIndex !== -1) {
            // Создаем новый массив, чтобы Vue отслеживал изменения
            this.cart = this.cart.map((item, index) =>
                index === existingIndex ? { ...item, quantity: item.quantity + 1 } : item
            );
        } else {
            this.cart = [...this.cart, { ...product, quantity: 1 }];
        }
        },
        removeFromCart(productId) {
            this.cart = this.cart.map(item => 
                item.id === productId ? { ...item, quantity: item.quantity - 1 } : item
            ).filter(item => item.quantity > 0);
        },
        updateCart(updatedCart) {
            this.cart = updatedCart;
        },
        toggleCart() {
            this.isCartOpen = !this.isCartOpen; // Открыть/закрыть корзину
        }
    }
};
</script>








