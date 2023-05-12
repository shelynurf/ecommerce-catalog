<script>
import "../assets/style/page.css";
/* eslint-disable */
export default {
	name: "ProductDisplay",
	data() {
		return {
			isLoading: !1,
			index: 0,
			isProductAvailable: !1,
			product: {},
			dataFromAPI: {},
		};
	},
	methods: {
		async getProductFromAPI() {
			const api = await fetch(
				`https://fakestoreapi.com/products/${this.index}`
			);
			const response = await api.json();

			this.dataFromAPI = response;
			return this.dataFromAPI;
			console.log(this.dataFromAPI);
		},
		async getSingleProduct() {
			this.isLoading = !0;
			20 !== this.index ? this.index++ : (this.index = 1);
			let singleProduct = await this.getProductFromAPI();
			"men's clothing" === singleProduct.category ||
			"women's clothing" === singleProduct.category
				? ((this.product = {
						data: singleProduct,
				  }),
				  (this.isProductAvailable = !0))
				: (this.isProductAvailable = !1);
			this.isLoading = !1;
			console.log("response :", this.dataFromAPI);
		},
	},
	mounted() {
		this.getSingleProduct();
	},
	computed: {
		formatNumber() {
			return new Intl.NumberFormat("en-US", {
				style: "currency",
				currency: "USD",
			}).format(this.dataFromAPI.price);
			// console.log(this.formatNumber(123));
		},
	},
};
</script>

<template>
	<div class="card-layout">
		<div class="card-product">
			<div class="card-content">
				<div style="display: flex; align-items: center">
					<!-- <img src="../assets/image.png" /> -->
					<img :src="dataFromAPI.image" style="max-width: 300px" />
				</div>
				<div style="width: 100%">
					<div style="height: 70px">
						<h1 class="women-title">
							<!-- <h1 :class="["women's clothing" === dataFromAPI.category ? "women-title" : "men-title"]"> -->
							{{ dataFromAPI.title }}
						</h1>
					</div>
					<div
						class="subtitle"
						style="
							display: flex;
							justify-content: space-between;
							margin-top: 1rem;
							margin-bottom: 1rem;
						">
						<span>{{ dataFromAPI.category }}</span>
						<div style="display: flex; gap: 0.5rem">
							<span>{{ dataFromAPI.rating }}/5</span>
							<div style="gap: 4px; display: flex; align-items: baseline">
								<img src="../assets/Ellipse-5.png" />
								<img src="../assets/Ellipse-5.png" />
								<img src="../assets/Ellipse-5.png" />
								<img src="../assets/Ellipse-5.png" />
								<img src="../assets/Ellipse-5.png" />
							</div>
						</div>
					</div>
					<hr />
					<div class="description" style="height: 13rem">
						<p>{{ dataFromAPI.description }}</p>
					</div>
					<div>
						<hr />
						<p class="women-title">{{ formatNumber }}</p>
						<div style="display: flex; gap: 1rem">
							<button class="women-btn-buy">Buy Now</button>
							<button class="women-btn-next" @click="getSingleProduct">
								Next Product
							</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
	<img class="women" src="../assets/bg-pattern.png" />
</template>
