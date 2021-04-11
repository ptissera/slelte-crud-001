<script>
    import { createEventDispatcher } from 'svelte';

    export let product;
    const dispatch = createEventDispatcher();

	const onDeleteHandler = productId => {
		dispatch('delete', {
			productId
		});
	};
	const onEditHandler = productEdit => {
		dispatch('update', {
			product: productEdit
		});
	};
</script>

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

<style>
</style>
