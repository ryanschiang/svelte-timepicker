<script>
	import TimePicker from '$lib/TimePicker.svelte';

	let hours = 0;
	let minutes = 0;
</script>

<div class="container">
	<h1>Svelte Time Picker</h1>
	<p>A simple time picker component for Svelte.</p>
	<p>
		{hours !== undefined ? hours : '__'}:{minutes !== undefined ? minutes : '__'}
	</p>
	<div class="max-w-xs">
		<TimePicker
			onChange={(h, m) => {
				hours = h;
				minutes = m;
			}}
		/>
	</div>
</div>

<style>
	.container {
		max-width: 800px;
		margin: 0 auto;
	}
</style>
