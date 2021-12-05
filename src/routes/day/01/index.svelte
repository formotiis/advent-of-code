<script lang='ts'>

	import { Day01Real, Day01Test } from './dataset';

	const real = Day01Real;
	const test = Day01Test;

	let isRealDataset = false;

	function numberOfIncrements(array: number[]): number {
		let total = 0
		array.forEach((v, i) => {
			if (i !== 0) {
				total += (array[i-1] < array[i]) ? 1 : 0;
			}
		})
		return total;
	}

	function numberOfIncrementsAvg(array: number[]): number {
		let total = 0
		array.forEach((v, i) => {
			if (i > 2 && i < (array.length)) {
				total += (average(array, i-1) < average(array, i)) ? 1 : 0;
			}
		})
		return total;
	}

	function average(array, index) {
		return index > 0
			? ( index > 1
				? (array[index-2] + array[index-1] + array[index])
				:  (array[index-1] + array[index]))
			: array[index];
	}



</script>

<h1>Day 01</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{numberOfIncrements(isRealDataset ? real : test)}</p>
<p class="lead"> Results on: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{numberOfIncrementsAvg(isRealDataset ? real : test)}</p>
