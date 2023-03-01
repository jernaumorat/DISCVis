<script lang="ts">
	import lodash from 'lodash';
	const { isEqual } = lodash;

	import Graph from '../Graph.svelte';
	import d3ToPng from 'd3-svg-to-png';
	import Item from '../Item.svelte';

	const rand = () => Math.round(Math.random() * 100);
	let data = [
		// {n: "AS", t: 100, r: 0, b: 0, l: 0},
		// {n: "OP", t: 0, r: 100, b: 0, l: 0},
		// {n: "PA", t: 0, r: 0, b: 100, l: 0},
		// {n: "CO", t: 0, r: 0, b: 0, l: 100},
	];
	let labels = {
		t: 'Assertive',
		r: 'Openness',
		b: 'Passive',
		l: 'Control'
	};
	let corners = {
		tl: 'D',
		tr: 'I',
		br: 'S',
		bl: 'C'
	};

	let [labelOn, cornerOn, axesOn] = [true, true, true];

	// const blankItem = () => ({n: "", t: rand(), r: rand(), b: rand(), l: rand()})
	const blankItem = () => ({ n: '', x: 0, y: 0, lr: 'left', col: '#f8f8ff' });
</script>

<Graph bind:data bind:labels bind:corners bind:labelOn bind:cornerOn bind:axesOn />
<div class="data">
	<div class="buttons">
		<button on:click={() => (data = [blankItem(), ...data])}>Add Item</button>
		<button
			on:click={() =>
				d3ToPng('svg', 'chart', {
					scale: 5,
					format: 'png',
					quality: 1
				})}
			>Save Image
		</button>
		<div>
			<label for="labelOn">Axis Labels</label>
			<input type="checkbox" id="labelOn" bind:checked={labelOn} />
		</div>
		<div>
			<label for="cornerOn">Corner Labels</label>
			<input type="checkbox" id="cornerOn" bind:checked={cornerOn} />
		</div>
		<div>
			<label for="axesOn">Axes</label>
			<input type="checkbox" id="axesOn" bind:checked={axesOn} />
		</div>
	</div>
	{#each data as item}
		<Item
			bind:labels
			bind:item
			callback={() => {
				console.log(data);
				data = data.filter((i) => !isEqual(i, item));
			}}
		/>
	{/each}
</div>

<style>
	:global(body) {
		font-family: sans-serif;
	}

	.buttons {
		display: flex;
		flex-direction: row;
		justify-content: space-evenly;
	}

	.buttons > button {
		width: 10vw;
	}

	.data {
		display: flex;
		flex-direction: column;
		height: 98vh;
		overflow: scroll;
		overflow-x: hidden;
	}
</style>
