<script lang='ts'>

	import { Day07Real, Day07Test } from './dataset';

	const real = Day07Real;
	const test = Day07Test;

	let isRealDataset = false;

	function calcLowestDst(dataset: number[]) {
		let min;

		dataset.forEach(
			crab => {
				const value =  dataset.map(c => Math.max(c, crab) - Math.min(c, crab)).reduce((p, c)=> p+c);
				if (!min ) {
					min = value;
				} else if (value < min){
					min = value;
				}
			}
		);

		return min;
	}

	function calcDiff(x: number): number {
		if (x < 2) {
			return x;
		}
		return (x + calcDiff(x-1));
	}

	function calcLowestDstCrabEdition(dataset: number[]) {
		const pseudoDataset = Array.from({length:Math.max(...dataset)},(v,k)=>k+1);
		let min;

		pseudoDataset.forEach(
			crab => {
				const value =  dataset.map(c => calcDiff(Math.max(c, crab) - Math.min(c, crab))).reduce((p, c)=> p+c);
				if (!min ) {
					min = value;
				} else if (value < min){
					min = value;
				}
			}
		);

		return min;
	}
</script>



<h1>Day 07</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calcLowestDst(isRealDataset ? real : test)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calcLowestDstCrabEdition(isRealDataset ? real : test)}</p>
