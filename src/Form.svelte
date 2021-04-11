<script>
    import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();
    const PRODUCT = {
			id: "",
			name: "",
			description: "",
			category: "laptops",
			imageURL: "",
		};
	export let product = {...PRODUCT};
	const cleanProduct = () => {
		product = {...PRODUCT};
	};
    const onSubmitHandler = () => {
        dispatch('submit', {
			product
		});
        cleanProduct();
    }
</script>

<div class="card bg-primary shadow-soft border-light mt-3">
    <div class="card-body">
        <form on:submit|preventDefault="{onSubmitHandler}">
            <h4>{product.id?'Update': 'Create'} product</h4>
            <div class="form-group">
                <input
                    type="text"
                    class="form-control"
                    bind:value={product.name}
                    placeholder="Product name"
                    required
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

<style>
</style>