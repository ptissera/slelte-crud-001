<script>
	import { v4 as uuidv4 } from "uuid";
	import Noty from "noty";
	import { onMount } from 'svelte';

	import "noty/lib/noty.css";
	import "noty/lib/themes/sunset.css";
	import Form from "./Form.svelte";
	import CardItem from "./CardItem.svelte";

	const store = window.localStorage;

	let products = [];

	onMount(async () => {
		const productsString = store.getItem("products");
		if (productsString == null) {
			products = [
				{
					id: 1,
					name: "HP Pavilion Notebook",
					description: "HP Laptop",
					imageURL: "https://product-images.www8-hp.com/digmedialib/prodimg/lowres/c06105651.png",
					category: "laptops",
				},
				{
					id: 2,
					name: "Mouse Razer",
					description: "gaming mouse",
					imageURL: 'https://oc-games.com.ar/image/cache/data/Mouse%20Razer%20Atheris%20Bluetooth-500x500.jpg',
					category: "peripherials",
				},
			];
		} else {
			products = JSON.parse(productsString);
		}
	});

	const onSubmitHandler = (event) => {
		const { product } = event.detail;
		if (!product.id) {
			const newProduct = { ...product, id: uuidv4() };
			products = [...products, newProduct];
		} else {
			const index = products.findIndex(({ id }) => id === product.id);
			products[index] = product;
		}
		store.setItem("products", JSON.stringify(products));
		new Noty({
			theme: "sunset",
			type: "success",
			timeout: 3000,
			text: `Product ${product.id ? "updated" : "created"} successfully`,
		}).show();
	};
	const onDeleteHandler = (event) => {
		const { productId } = event.detail;
		products = products.filter(({ id }) => id !== productId);
		store.setItem("products", JSON.stringify(products));
		new Noty({
			theme: "sunset",
			type: "success",
			timeout: 3000,
			text: `Product deleted successfully`,
		}).show();
	};
	let productEdit = {};
	const onEditHandler = (event) => {
		productEdit = event.detail.product;
	};
</script>

<main>
	<div class="row m-0">
		<div class="col-md-1" />
		<div class="col-md-5">
			{#each products as product}
				<CardItem
					{product}
					on:delete={onDeleteHandler}
					on:update={onEditHandler}
				/>
			{/each}
		</div>
		<div class="col-md-5">
			<Form product={productEdit} on:submit={onSubmitHandler} />
		</div>
		<div class="col-md-1" />
	</div>
</main>

<style>
</style>
