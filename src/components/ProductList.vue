<template>
    <div>
        <h1>Product List</h1>
        <img
            v-if="loading" 
            src="https://i.imgur.com/JfPpwOA.gif">
        <ul v-else>
            <li 
                v-for="product in products"
                :key="product['id']">
                {{product.title}} - {{product.price | currency}} - {{product.inventory}}
                <button
                    :disabled="!productIsInStock(product)" 
                    @click="addProductToCart(product)"
                    >add to cart</button>
            </li>
        </ul>
    </div>
</template>

<script>
import {mapState, mapGetters, mapActions} from 'vuex'

export default {
    data () {
        return {
            loading: false
        }
    },    
    computed: {
        ...mapState({
           products: state => state.products.items
        }),
        ...mapGetters('products',{
            productIsInStock: 'productIsInStock'
        })
        // ,
        // productIsInStock (){
        //     return this.$store.getters.productIsInStock
        // }
    },
    methods: {
        ...mapActions({
            fetchProducts: 'products/fetchProducts',
            addProductToCart: 'cart/addProductToCart'
        })
        // ,
        // addProductToCart(product){
        //     this.$store.dispatch('addProductToCart', product)
        // }
    },


    // computed: {
    //     products () {
    //         return this.$store.state.products
    //     },
    //     productIsInStock (){
    //         return this.$store.getters.productIsInStock
    //     }
    // },
    created () {
        this.loading = true
        //this.$store.dispatch('products/fetchProducts')
        this.fetchProducts()
        .then(() => this.loading = false)
    }
}
</script>
