<script lang="ts">
	import { tick } from 'svelte';

	let selected = -1;
	let compared = -1;
	let sortedIndices: number[] = [];

	// Asynchronous bubble sort
	const bubbleSort = async (arr: number[]): Promise<number[]> => {
		const len = arr.length;
		let sortedArray = [...arr]; // Copy the array to avoid direct mutation

		for (let i = 0; i < len; i++) {
			let isSwapped = false;
			for (let j = 0; j < len - i - 1; j++) {
				selected = j;
				compared = j + 1;

				if (sortedArray[j] > sortedArray[j + 1]) {
					const temp = sortedArray[j];
					sortedArray[j] = sortedArray[j + 1];
					sortedArray[j + 1] = temp;
					isSwapped = true;

					nums = [...sortedArray]; // Update nums with the sorted array

					// Visualize the sorting step
					await visualiseSortingStep();
				}
			}

			// If no two elements were swapped by inner loop, then the array is sorted
			if (!isSwapped) break;

			// Mark the last element of the current iteration as sorted
			sortedIndices.push(len - i - 1);
		}

		// Reset the selected and compared indices after sorting
		selected = -1;
		compared = -1;

		return sortedArray;
	};

	const visualiseSortingStep = (): Promise<void> => {
		return new Promise((resolve) => {
			setTimeout(async () => {
				await tick(); // This ensures Svelte updates the DOM
				resolve();
			}, 30); // 100ms delay between each step
		});
	};

	// Returns unsorted array of random numbers
	const generateNumsArray = (arrayLength: number): number[] => {
		let numsArray = [];

		// Populate numsArray with "arrayLength" random numbers
		for (let i = 1; i <= arrayLength; i++) {
			const randomNumber = Math.floor(Math.random() * 100) + 1;
			numsArray.push(randomNumber);
		}

		return numsArray;
	};

	// Initialize unsorted list of random numbers
	let numberOfBars = 40;
	let nums = generateNumsArray(numberOfBars);

	const startClickedHandler = async () => {
		await bubbleSort(nums);
	};

	const resetClickedHandler = () => {
		nums = generateNumsArray(numberOfBars);
		sortedIndices = [];
	};

	// Returns height for the bar
	const getBarHeight = (num: number) => {
		return num * 4; // Adjust the scaling factor as needed for visual representation
	};
</script>

<main class="text-center">
	<h1 class="text-2xl font-bold">Bubble Sort: A Sorting Algorithm</h1>
	<p>This application visualizes the patterns created by Bubble Sort</p>
	<div class="h-2" />
	<div class="flex flex-row justify-center gap-2 items-center ml-auto mr-auto">
		<button
			class="w-28 border-[1px] rounded-md border-black p-1 active:bg-black active:text-white"
			on:click={startClickedHandler}>Start</button
		>
		<button
			class="w-28 border-[1px] rounded-md border-black p-1 active:bg-black active:text-white"
			on:click={resetClickedHandler}>Reset</button
		>
	</div>
	<div class="h-6" />
	<div class="container border-2 w-min p-2 flex flex-row items-end gap-1 mr-auto ml-auto">
		{#each nums as num, index}
			<div
				style={`height: ${getBarHeight(num)}px;`}
				class={`bg-blue-300 w-3 flex flex-col justify-end items-center pb-1 
               ${index === selected || index === compared ? 'bg-green-300' : ''}
               ${sortedIndices.includes(index) ? 'bg-purple-300' : ''}`}
			>
				<p style="transform: rotate(90deg);" class="text-sm text-blue-700">{num}</p>
			</div>
		{/each}
	</div>
</main>
