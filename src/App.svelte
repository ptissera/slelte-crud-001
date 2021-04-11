<script>
	import { v4 as uuidv4 } from 'uuid';
	import Noty from 'noty';

	import 'noty/lib/noty.css';
	import 'noty/lib/themes/sunset.css';
import Form from './Form.svelte';
import CardItem from './CardItem.svelte';

	let products = [
		{
			id: 1,
			name: "HP Pavilion Notebook",
			description: "HP Laptop",
			category: "laptops",
		},
		{
			id: 2,
			name: "Mouse Razer",
			description: "gaming mouse",
			category: "peripherials",
		},
	];
	const onSubmitHandler = (event) => {
		const {product} = event.detail;
		if (!product.id) {
			const newProduct = { ...product, id: uuidv4() };
			products = [...products, newProduct];
		} else {
			const index = products.findIndex(({ id }) => id === product.id)
			products[index] =product;
		}
		new Noty({
				theme: 'sunset',
				type: 'success',
				timeout: 3000,
				text: `Product ${product.id ? 'updated' : 'created' } successfully`
			}).show();
	};
	const onDeleteHandler = (event) => {
		const {productId} =event.detail;
		products = products.filter(({ id }) => id !== productId);
		new Noty({
				theme: 'sunset',
				type: 'success',
				timeout: 3000,
				text: `Product deleted successfully`
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
				<CardItem {product} on:delete={onDeleteHandler} on:update={onEditHandler}/>
			{/each}
		</div>
		<div class="col-md-5">
			<Form product={productEdit} on:submit={onSubmitHandler}></Form>
		</div>
		<div class="col-md-1" />
	</div>
</main>

<style>
</style>
