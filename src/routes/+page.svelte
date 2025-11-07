<script lang="ts">
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	let RevoGrid: any = null;

	// Type for our data items
	interface DataItem {
		name: string;
		price: number;
		quantity: number;
		total: number;
	}

	// Simple data for RevoGrid
	let data: DataItem[] = [
		{ name: 'Apple', price: 2.5, quantity: 10, total: 25 },
		{ name: 'Banana', price: 1.2, quantity: 15, total: 18 },
		{ name: 'Orange', price: 3.0, quantity: 8, total: 24 },
		{ name: 'Grapes', price: 4.5, quantity: 5, total: 22.5 },
		{ name: 'Mango', price: 2.8, quantity: 12, total: 33.6 }
	];

	// Reactive calculation - recalculate totals whenever data changes
	$: {
		data.forEach((item, index) => {
			const newTotal = item.price * item.quantity;
			if (item.total !== newTotal) {
				data[index] = { ...item, total: newTotal };
			}
		});
	}

	// Column definitions - very simple!
	const columns = [
		{ prop: 'name', name: 'Product', size: 150 },
		{ prop: 'price', name: 'Price ($)', columnType: 'numeric', size: 100 },
		{ prop: 'quantity', name: 'Quantity', columnType: 'numeric', size: 100 },
		{ prop: 'total', name: 'Total ($)', columnType: 'numeric', size: 120, readonly: true }
	];

	// Load RevoGrid dynamically
	onMount(async () => {
		if (browser) {
			try {
				const module = await import('@revolist/svelte-datagrid');
				RevoGrid = module.RevoGrid;
			} catch (error) {
				console.error('RevoGrid loading failed:', error);
			}
		}
	});

	// Handle cell edits for calculations
	function onAfterEdit(event: any) {
		const { detail } = event;
		const { rowIndex, prop, val } = detail;
		
		if ((prop === 'price' || prop === 'quantity') && data[rowIndex]) {
			const numValue = parseFloat(val) || 0;
			
			// Update the specific property
			if (prop === 'price') {
				data[rowIndex].price = numValue;
			} else if (prop === 'quantity') {
				data[rowIndex].quantity = numValue;
			}
			
			// Recalculate total
			data[rowIndex].total = data[rowIndex].price * data[rowIndex].quantity;
			
			// Trigger reactivity
			data = [...data];
		}
	}
</script>

<svelte:head>
	<title>Simple RevoGrid Calculator</title>
	<meta name="description" content="A simple data grid with math calculations" />
</svelte:head>

<div class="container">
	<h1>RevoGrid Calculator</h1>
	<p>Edit price and quantity to see automatic total calculations.</p>
	
	{#if browser && RevoGrid}
		<div class="grid-wrapper">
			<svelte:component 
				this={RevoGrid}
				source={data}
				{columns}
				theme="compact"
				on:afteredit={onAfterEdit}
			/>
		</div>
	{:else if browser}
		<div class="loading">
			<p>Loading RevoGrid... 🚀</p>
		</div>
	{:else}
		<p>RevoGrid loads in the browser</p>
	{/if}

	<div class="summary">
		<h3>Summary</h3>
		<p><strong>Grand Total: ${data.reduce((sum, item) => sum + item.total, 0).toFixed(2)}</strong></p>
		<p>Total Items: {data.reduce((sum, item) => sum + item.quantity, 0)}</p>
	</div>
</div>

<style>
	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 1rem;
	}

	h1 {
		color: #333;
		text-align: center;
		margin-bottom: 0.5rem;
	}

	p {
		text-align: center;
		color: #666;
		margin-bottom: 2rem;
	}

	.grid-wrapper {
		margin: 2rem 0;
		border-radius: 8px;
		overflow: hidden;
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
		background: white;
		min-height: 400px;
	}

	.loading {
		text-align: center;
		padding: 4rem 2rem;
		color: #666;
		font-size: 1.2rem;
	}

	.summary {
		background: #f8f9fa;
		padding: 1rem;
		border-radius: 8px;
		margin-top: 2rem;
		text-align: center;
	}

	.summary h3 {
		margin-top: 0;
		color: #333;
	}

	.summary p {
		margin: 0.5rem 0;
		font-size: 1.1rem;
	}



	/* Mobile responsive */
	@media (max-width: 768px) {
		.container {
			padding: 0.5rem;
		}
		
		h1 {
			font-size: 1.5rem;
		}
		
		.grid-wrapper {
			margin: 1rem 0;
		}
		
		.summary {
			padding: 0.75rem;
		}
	}
</style>