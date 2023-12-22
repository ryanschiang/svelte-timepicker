<script lang="ts">
	export let hours: number | null = null;
	export let minutes: number | null = null;
	let isOpen = false;
	let hoursCol: HTMLDivElement;
	let minutesCol: HTMLDivElement;

	export let onChange: (hours: number, minutes: number) => void = () => {};

	function clickAway(node: HTMLElement, callbackFunction: () => void) {
		function onClick(event: MouseEvent) {
			if (!node.contains(event.target as Node)) {
				callbackFunction();
			}
		}

		document.body.addEventListener('click', onClick);

		return {
			update(newCallbackFunction: () => void) {
				callbackFunction = newCallbackFunction;
			},
			destroy() {
				document.body.removeEventListener('click', onClick);
			}
		};
	}

	const addLeadingZero = (num: number | null) => {
		if (num === null) return '';
		return num < 10 ? `0${num}` : num;
	};

	$: scrollToTime = () => {
		if (hoursCol && minutesCol) {
			const hourButton = hoursCol.querySelector(
				`#hour-${hours && hours > 12 ? hours - 12 : hours}`
			);
			if (hourButton)
				hourButton.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'start' });
			const minutesButton = minutesCol.querySelector(`#min-${minutes}`);
			if (minutesButton)
				minutesButton.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'start' });
		}
	};

	$: handleHoursClick = (hour: number) => {
		if (hours === null) {
			hours = hour + 12 > 12 ? hour : hour + 12;
		} else {
			if (hours >= 12) {
				hours = hour + 12;
			} else {
				hours = hour;
			}
		}
	};

	$: handleMinutesClick = (min: number) => {
		minutes = min * 5;
	};

	$: handleAMClick = () => {
		hours = hours !== null && hours >= 12 ? hours - 12 : hours;
	};

	$: handlePMClick = () => {
		hours = hours !== null && hours < 12 ? hours + 12 : hours;
	};

	$: {
		if (hours !== null && minutes !== null) {
			onChange(hours, minutes);
			scrollToTime();
		}
	}
</script>

<div
	class="container"
	use:clickAway={() => {
		isOpen = false;
	}}
>
	<input
		type="time"
		class="input"
		on:input={(e) => {
			const split = e.currentTarget.value.split(':');
			hours = parseInt(split[0]);
			if (isNaN(hours)) hours = null;
			minutes = parseInt(split[1]);
			if (isNaN(minutes)) minutes = null;
		}}
		on:focus={(e) => {
			isOpen = true;
		}}
		value="{addLeadingZero(hours)}:{addLeadingZero(minutes)}"
	/>

	{#if isOpen}
		<div class="popover">
			<div class="popover_header">
				<div bind:this={hoursCol} class="popover_header_col">
					{#each Array.from(Array(12)) as _, hour}
						<button
							id={`hour-${hour + 12 > 12 ? hour : hour + 12}`}
							class="popover_header_btn"
							class:active={hours === 0
								? hour === 0
								: hours &&
									(hours > 12 ? hours - 12 : hours) === (hour + 12 > 12 ? hour : hour + 12)}
							on:click={() => handleHoursClick(hour)}
						>
							{hour + 12 > 12 ? addLeadingZero(hour) : addLeadingZero(hour + 12)}
						</button>
					{/each}
				</div>
				<div bind:this={minutesCol} class="popover_header_col">
					{#each Array.from(Array(12)) as _, min}
						<button
							id={`min-${min * 5}`}
							class="popover_header_btn"
							class:active={minutes === min * 5}
							on:click={() => handleMinutesClick(min)}
						>
							{addLeadingZero(min * 5)}
						</button>
					{/each}
				</div>
				<div class="popover_header_col">
					<button
						class="popover_header_btn"
						class:active={hours !== null && minutes !== null && hours < 12}
						on:click={handleAMClick}
					>
						AM
					</button>
					<button
						class="popover_header_btn"
						class:active={hours !== null && minutes !== null && hours >= 12}
						on:click={handlePMClick}
					>
						PM
					</button>
				</div>
			</div>
			<div class="popover_footer">
				<button
					class="popover_footer_btn"
					on:click={() => {
						isOpen = false;
					}}>OK</button
				>
			</div>
		</div>
	{/if}
</div>

<style>
	.container {
		position: relative;
		display: inline-block;
		width: 100%;
		max-width: 20rem;
	}

	.input {
		padding: 0.375rem 0.5rem;
		width: 100%;
		max-width: 20rem;
		border-radius: 0.375rem;
		color: black;
		border: 1px solid #ccc;
		font-size: 14px;
	}

	.input:focus {
		border-color: blue;
		outline: 1px solid blue;
	}

	.popover {
		background: white;
		position: absolute;
		z-index: 10;
		border-radius: 0.375rem;
		border: 1px solid #ccc;
	}

	.popover_header {
		display: flex;
		flex-direction: row;
	}

	.popover_header_col {
		display: flex;
		flex-direction: column;
		padding: 0.5rem 0.325rem;
		max-height: 10rem;
		overflow-y: scroll;
	}

	.popover_header_col:not(:last-child) {
		border-right: 1px solid #ccc;
	}

	.popover_header_btn {
		padding: 0.5rem 1rem;
		background: transparent;
		border: none;
		font-size: 14px;
		cursor: pointer;
		color: black;
	}

	.popover_header_btn.active {
		background: #165fc7;
		color: white;
	}
	.popover_header_btn:not(.active):hover {
		background: hsl(0, 0%, 93%);
	}

	.popover_footer {
		display: flex;
		justify-content: flex-end;
		padding: 0.5rem 1rem;
		border-top: 1px solid #ccc;
	}

	.popover_footer_btn {
		color: blue;
		font-weight: 500;
		background: transparent;
		border: none;
		cursor: pointer;
		font-size: 14px;
	}
</style>
