<script lang="ts">
	let width = 10;
	let height = 8;
	let running = false;
	$: btnText = running ? 'Stop animation' : 'Start animation';
	$: colors = new Array<string>(width * height); //.fill('green');
	let frame = 0;

	const step = () => {
		frame++;
		const newColors = new Array<string>(width * height);
		for (let y = 0; y < height; y++) {
			for (let x = 0; x < width; x++) {
				const red = (((x + frame) * 256) / width) % 256;
				const blue = (((y + frame) * 256) / height) % 256;
				newColors[x + y * width] = `rgb(${red}, 0, ${blue})`;
			}
		}
		colors = newColors;
		running && requestAnimationFrame(step);
	};

	$: ((r) => {
		console.log('Toggle running');
		r && requestAnimationFrame(step);
	})(running);
</script>

<button
	on:click={() => {
		running = !running;
	}}>{btnText}</button
>
<p>
	<label>Width: <input type="number" max="50" min="10" bind:value={width} /></label>
	<label>Height: <input type="number" min="4" max="20" bind:value={height} /></label>
</p>
<div class="lightsContainer">
	{#each new Array(height) as _, row}
		<div class="row">
			{#each new Array(width) as _, col}
				<div
					on:mouseenter={(e) => {
						if (e.buttons > 0) colors[row * width + col] = 'white';
					}}
					class="light"
					style={`background-color: rgb(${(col * 256) / width}, 0, ${
						(row * 256) / height
					}); background-color: ${colors[row * width + col]}; `}
				/>
			{/each}
		</div>
	{/each}
</div>

<style>
	.lightsContainer {
		background-color: black;
	}
	.row {
		display: flex;
		flex-direction: row;
	}
	.light {
		width: 1vw;
		height: 1vw;
		/* border: 1px solid black; */
	}
</style>
