<template>
    <div>
        <h1>Product List</h1>
        <img
        	v-if="loading"
        	src="https://i.imgur.com/JfPpwOA.gif"
        >
        <ul v-else>
           <li v-for="product in products">
           		{{product.title}} - {{product.price | currency}} - {{product.inventory}}
           		<button 
           		:disabled="!productIsInStock(product)"
           		@click="addProductToCart(product)"
           		>

           		Add to Cart</button>
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
			...mapState('product',{
		 		products: state => state.items
		 	}),

		 	...mapGetters('product',{
		 		productIsInStock: 'productIsInStock'
		 	})

			// products () {
			// 	return this.$store.state.products
			// },

			// productIsInStock () {
			// 	return this.$store.getters.productIsInStock
			// }
		},

		methods: {
			...mapActions({
				fetchProducts: 'product/fetchProducts',
				addProductToCart: 'cart/addProductToCart'
			}),

			// addProductToCart (product) {
			// 	this.$store.dispatch('addProductToCart', product)
			// }
		},

		created () {
			this.loading = true
			this.fetchProducts()
				.then(() => this.loading = false)
		}
	};
</script>

<style scoped>
</style>
