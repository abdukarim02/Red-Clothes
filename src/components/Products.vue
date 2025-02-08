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
                    :image="product.imag"
                    :favourite="product.favourite"
                    :title="product.title"
                    :text="product.text"
                    :price="product.price"
                    @add-to-cart="addToCart"
                />
            </div>
            <Basket :cart="cart" @remove-from-cart="addToCart" />
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
            products: [
                { id: 1, imag: "/src/img/products-1.png", favourite: "/src/img/favourite-products-1.png", title: "Blacksi", text: "Костюм спортивный", price: 3595 },
                { id: 2, imag: "/src/img/products-2.png", favourite: "/src/img/favourite-products-1.png", title: "Fashion.Love.Story", text: "Платье", price: 3500 },
                { id: 3, imag: "/src/img/products-3.png", favourite: "/src/img/favourite-products-1.png", title: "UNIQLO", text: "Водолазка", price: 2999 },
                { id: 4, imag: "/src/img/products-4.png", favourite: "/src/img/favourite-products-1.png", title: "Mark Formelle", text: "Костюм домашний", price: 1699 },
                { id: 5, imag: "/src/img/products-5.png", favourite: "/src/img/favourite-products-1.png", title: "Vittoria Vicci", text: "Пуловер", price: 3595 },
                { id: 6, imag: "/src/img/products-6.png", favourite: "/src/img/favourite-products-1.png", title: "O'stin", text: "Платье", price: 3799 }
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
            const existingProduct = this.cart.find(item => item.id === product.id);
            if (existingProduct) {
                existingProduct.quantity++;
            } else {
                this.cart.push({ ...product, quantity: 1 });
            }
        },
        removeFromCart(productId) {
            this.cart = this.cart.filter(item => item.id !== productId);
        }
    }
};
</script>





