<script lang='ts'>

	import { Day01Real, Day01Test } from './dataset';

	const real = Day01Real;
	const test = Day01Test;

	let isRealDataset = false;

	function sortedTotals(input: string){
		const packs: string[] = input.split("\n\n");
		const totals: number[] = packs.map(pack => pack.split("\n").map(string => +string).reduce((a,c) => a + c))
		return totals.sort((a, b) => b - a);
	}

	function mostCalories(input: string): number {
		return sortedTotals(input)[0];
	}

	function mostCalories3(input: string): number {
		return sortedTotals(input)[0] +sortedTotals(input)[1] + sortedTotals(input)[2];
	}



</script>

<h1>Day 01</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{mostCalories(isRealDataset ? real : test)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{mostCalories3(isRealDataset ? real : test)}</p>
