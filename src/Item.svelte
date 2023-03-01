<script>
	import ColorPicker from 'svelte-awesome-color-picker';

	export let labels;
	export let item;
	export let callback;
</script>

<div class="item">
	<div class="outer">
		<label for="initial">Initials</label>
		<input id="initial" type="text" bind:value={item.n} />
	</div>
	<div class="outer">
		<div>
			<label for="radLeft">{labels.l}</label>
			<input name="leftright" id="radLeft" value="left" type="radio" bind:group={item.lr} />
			<label for="radRight">{labels.r}</label>
			<input name="leftright" id="radRight" value="right" type="radio" bind:group={item.lr} />
		</div>
	</div>
	<div class="outer">
		<ColorPicker label="" isAlpha={false} bind:hex={item.col} />
	</div>
	<div>
		<div>
			<label for="x">Horizontal</label>
			<input id="x" type="number" max="100" min="-100" bind:value={item.x} />
		</div>
		<input type="range" max="100" min="-100" tabindex="-1" bind:value={item.x} />
	</div>
	<div>
		<div>
			<label for="y">Vertical</label>
			<input id="y" type="number" max="100" min="-100" bind:value={item.y} />
		</div>
		<input type="range" max="100" min="-100" tabindex="-1" bind:value={item.y} />
	</div>
	<button on:click={callback}>Delete</button>
</div>

<style>
	.item {
		display: flex;
		flex-direction: row;
		justify-content: space-evenly;
		margin: 5px;
		padding: 5px;
	}

	:global(.color) {
		border: 1px solid black;
	}

	.outer {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.item > button {
		margin: 0 5px 0 5px;
	}

	input {
		width: 2em;
		margin: 0 5px 0 5px;
	}

	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
		/* display: none; <- Crashes Chrome on hover */
		-webkit-appearance: none;
		margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
	}

	input[type='number'] {
		-moz-appearance: textfield; /* Firefox */
	}

	input[type='range'] {
		width: 100%;
	}
</style>

