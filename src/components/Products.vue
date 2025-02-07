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
                    :imag="product.imag"
                    :favourite="product.favourite"
                    :title="product.title"
                    :text="product.text"
                    :price="product.price"
                />
            </div>
        </div>
    </section>
</template>

<script>
import ProductsList from './ProductsList.vue';

export default {
    components: {
        ProductsList
    },
    data() {
        return {
            searchQuery: "", // Поле ввода поиска
            products: [
                {
                    id: 1,
                    imag: "/src/img/products-1.png",
                    favourite: "/src/img/favourite-products-1.png",
                    title: "Blacksi",
                    text: "Костюм спортивный",
                    price: "3 595 ₽"
                },
                {
                    id: 2,
                    imag: "/src/img/products-2.png",
                    favourite: "/src/img/favourite-products-1.png",
                    title: "Fashion.Love.Story",
                    text: "Платье",
                    price: "3 500 ₽"
                },
                {
                    id: 3,
                    imag: "/src/img/products-3.png",
                    favourite: "/src/img/favourite-products-1.png",
                    title: "UNIQLO",
                    text: "Водолазка",
                    price: "2 999 ₽"
                },
                {
                    id: 4,
                    imag: "/src/img/products-4.png",
                    favourite: "/src/img/favourite-products-1.png",
                    title: "Mark Formelle",
                    text: "Костюм домашний",
                    price: "1 699 ₽"
                },
                {
                    id: 5,
                    imag: "/src/img/products-5.png",
                    favourite: "/src/img/favourite-products-1.png",
                    title: "Vittoria Vicci",
                    text: "Пуловер",
                    price: "3 595 ₽"
                },
                {
                    id: 6,
                    imag: "/src/img/products-6.png",
                    favourite: "/src/img/favourite-products-1.png",
                    title: "O'stin",
                    text: "Платье",
                    price: "3 799 ₽"
                }
            ]
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
            // Метод обновляет список `filteredProducts`, срабатывает на кнопку и ввод
            console.log("Поиск: ", this.searchQuery);
        }
    }
};
</script>


