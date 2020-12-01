<template>
    <div id="app">
        <div class="mx-auto mt-12 max-w-5xl">
            <div class="text-3xl mb-8">Menu</div>

            <div v-for="category in categories" :key="category.id" class="mb-20">
                <div class="text-2xl text-green-800 mb-2">{{ categoryDescription(category) }}</div>

                <div>
                    <div class="grid gap-6 grid-cols-3">
                        <div v-for="product in category.products" :key="product.id"
                             class="bg-gray-100 rounded-3xl shadow-lg">
                            <img :src="`https://cavanektar.gr${product.image_path}`"/>

                            <div class="p-4">
                                <div class="font-bold">{{ product.name.el }}</div>

                                <div class="mt-4">{{ product.description.el }}</div>

                                <div class="mt-4 text-right">{{ product.price | formatPrice }}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'App',

        data() {
            return {
                categories: []
            }
        },

        methods: {
            categoryDescription(category) {
                if (!category.name.en) {
                    return category.name.el;
                }

                return `${category.name.el} / ${category.name.en}`;
            }
        },

        filters: {
            formatPrice(price) {
                return `${price.toFixed(2)}€`;
            }
        },

        mounted() {
            axios.get('https://cavanektar.gr/api/categories')
                .then(response => {
                    this.categories = response.data;
                })
                .catch(error => {
                    console.log(error);
                });
        }
    }
</script>


