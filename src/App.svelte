<script>
	import { v4 as uuidv4 } from 'uuid';
	import Noty from 'noty';

	import 'noty/lib/noty.css';
	import 'noty/lib/themes/sunset.css';

	let products = [
		{
			id: 1,
			name: "HP Pavilion Notebook",
			description: "HP Laptop",
			category: "laptop",
		},
		{
			id: 2,
			name: "Mouse Razer",
			description: "gaming mouse",
			category: "peripherials",
		},
	];
	let product = {
		id: "",
		name: "",
		description: "",
		category: "",
		imageURL: "",
	};
	const onSubmitHandler = () => {
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
		cleanProduct();
	};
	const cleanProduct = () => {
		product = {
			id: "",
			name: "",
			description: "",
			category: "",
			imageURL: "",
		};
	};
	const onDeleteHandler = productId => {
		products = products.filter(({ id }) => id !== productId);
		new Noty({
				theme: 'sunset',
				type: 'success',
				timeout: 3000,
				text: `Product deleted successfully`
			}).show();
	};
	const onEditHandler = productEdit => {
		product = { ...productEdit };
	};
</script>

<main>
	<div class="row m-0">
		<div class="col-md-1" />
		<div class="col-md-5">
			{#each products as product}
				<div class="card bg-primary shadow-soft border-light mt-3">
					<div class="row mt-2">
						<div class="col-md-4">
							{#if !product.imageURL}
								<img
									src="assets/img/no_products_found.png"
									alt=""
									class="img-fluid p-2"
								/>
							{:else}
								<img
									src={product.imageURL}
									alt=""
									class="img-fluid p-2"
								/>
							{/if}
						</div>
						<div class="col-md-8">
							<div class="card-body">
								<h5>
									{product.name}
									<strong>
										<small>{product.category}</small>
									</strong>
								</h5>
								<p class="card-text">{product.description}</p>
								<button
									class="btn btn-danger"
									on:click={onDeleteHandler(product.id)}
									>Delete</button
								>
								<button
									class="btn btn-secondary"
									on:click={onEditHandler(product)}
									>Edit</button
								>
							</div>
						</div>
					</div>
				</div>
			{/each}
		</div>
		<div class="col-md-5">
			<div class="card bg-primary shadow-soft border-light mt-3">
				<div class="card-body">
					<form on:submit|preventDefault={onSubmitHandler}>
						<h4>Create product</h4>
						<div class="form-group">
							<input
								type="text"
								class="form-control"
								bind:value={product.name}
								placeholder="Product name"
								id="product-name"
							/>
						</div>
						<div class="form-group">
							<textarea
								bind:value={product.description}
								type="text"
								class="form-control"
								rows="2"
								placeholder="Product description"
								id="product-description"
							/>
						</div>
						<div class="form-group">
							<input
								type="url"
								class="form-control"
								bind:value={product.imageURL}
								placeholder="Image URL"
							/>
						</div>
						<div class="form-group">
							<select
								id="category"
								class="form-control"
								bind:value={product.category}
							>
								<option value="laptops">Laptops</option>
								<option value="peripherials"
									>Feripherials</option
								>
								<option value="servers">Servers</option>
							</select>
						</div>
						<button class="btn btn-primary"> Guardar </button>
					</form>
				</div>
			</div>
		</div>
		<div class="col-md-1" />
	</div>
</main>

<style>
</style>
