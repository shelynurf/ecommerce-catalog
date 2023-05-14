<script>
import "../assets/style/page.css";
/* eslint-disable */
export default {
	name: "ProductDisplay",
	data() {
		return {
			isLoading: false,
			index: 0,
			isProductAvailable: false,
			product: {},
			dataFromAPI: {},
			womens: false,
			mens: false,
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
			this.isLoading = true;

			if (this.index !== 20) {
				this.index++;
			} else {
				this.index = 1;
			}
			let singleProduct = await this.getProductFromAPI();
			this.mens = singleProduct.category === "men's clothing";
			this.womens = singleProduct.category === "women's clothing";
			this.mens || this.womens
				? ((this.product = {
						data: singleProduct,
				  }),
				  (this.isProductAvailable = true))
				: (this.isProductAvailable = false);
			this.isLoading = false;
			console.log("response :", this.dataFromAPI);
			console.log("single product :", singleProduct);
			console.log("women", this.womens);
			console.log("men", this.mens);
			console.log(typeof bulletRating);
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
		},
	},
};
</script>

<template>
	<div class="card-layout">
		<div v-if="!isLoading" class="card-product">
			<div v-if="isProductAvailable" class="card-content">
				<div style="display: flex; align-items: center">
					<img :src="dataFromAPI.image" style="max-width: 300px" />
				</div>
				<div style="width: 100%">
					<div style="height: 70px">
						<h1 :class="womens === true ? 'women-title' : 'men-title'">
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
							<span>{{ this.dataFromAPI.rating.rate }}/5</span>
							<div style="gap: 4px; display: flex; align-items: baseline">
								<div
									:class="
										womens === true ? 'women-circle' : 'men-circle'
									"></div>
								<div
									:class="
										womens === true ? 'women-circle' : 'men-circle'
									"></div>
								<div
									:class="
										womens === true ? 'women-circle' : 'men-circle'
									"></div>
								<div
									:class="
										womens === true ? 'women-circle' : 'men-circle'
									"></div>
								<div
									:class="
										womens === true
											? 'women-circle-outline'
											: 'men-circle-outline'
									"></div>
							</div>
						</div>
					</div>
					<hr />
					<div class="description" style="height: 13rem">
						<p>{{ dataFromAPI.description }}</p>
					</div>
					<div>
						<hr />
						<p :class="womens === true ? 'women-title' : 'men-title'">
							{{ formatNumber }}
						</p>
						<div style="display: flex; gap: 1rem">
							<button :class="womens ? 'women-btn-buy' : 'men-btn-buy'">
								Buy Now
							</button>
							<button
								:class="womens === true ? 'women-btn-next' : 'men-btn-next'"
								@click="getSingleProduct()">
								Next Product
							</button>
						</div>
					</div>
				</div>
			</div>
			<div v-else>
				<div class="unv-bg">
					<img src="../assets/sad-face.png" />
				</div>
				<div class="unv-text">
					<p class="description" style="color: #000000; text-align: center">
						This product is unavailable to show
					</p>
					<button class="unv-btn-next" @click="getSingleProduct()">
						Next Product
					</button>
				</div>
			</div>
		</div>
		<div v-if="isLoading" class="loader"></div>
	</div>
	<img
		v-if="isProductAvailable"
		:class="womens === true ? 'bg-women' : 'bg-men'"
		src="../assets/bg-pattern.png" />
	<img v-else class="bg-unv" src="../assets/bg-pattern.png" />
</template>
