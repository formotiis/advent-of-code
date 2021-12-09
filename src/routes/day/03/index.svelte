<script lang="ts">


	import { Day03Real, Day03Test } from './dataset';

	const real = Day03Real;
	const test = Day03Test;

	let isRealDataset = false;




	function findGammaEps(dataset: string[]): number {
		let gamma = "";
		let epsilon = "";

		for (let i = 0; i < dataset[0].length; i++) {
			const isGammaOne = dataset.map(binary => +binary.charAt(i)).reduce((p, c) => p + c) > (dataset.length / 2);
			gamma = gamma.concat(isGammaOne ? '1' : '0');
			epsilon = epsilon.concat(isGammaOne ? '0' : '1');
		}
		return parseInt(gamma, 2) * parseInt(epsilon, 2);
	}

	function findOxyCO2(dataset: string[]): number {
		let o2 =  reccuFind(0, true, dataset);
		let co2 = reccuFind(0, false, dataset);

		return parseInt(o2, 2) * parseInt(co2, 2);
	}

	function reccuFind(charAt: number, most: boolean, dataset: string[]): string {
		if (!dataset  || dataset.length === 0|| charAt === dataset[0]?.length) {
			return ''
		} else {
			if (dataset.length === 1) {
				return dataset[0].charAt(charAt) + reccuFind(charAt + 1, most, dataset.slice());
			} else {
				const isOneMostLikely =
					(dataset.map(binary => +binary.charAt(charAt)).reduce((p, c) => p + c) >= (dataset.length / 2));

				const nextDataset =
					dataset.length === 1
						? dataset
						: (most
							? dataset.filter(line => line.charAt(charAt) === (isOneMostLikely ? '1' : '0'))
							: dataset.filter(line => line.charAt(charAt) === (isOneMostLikely  ? '0' : '1')));

				return (most
					? (isOneMostLikely ? '1' : '0')
					: (isOneMostLikely ? '0' : '1')) + reccuFind(charAt + 1, most, nextDataset.slice());
			}
		}
	}



</script>


<h1>Day 02</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{findGammaEps(isRealDataset ? real : test)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{findOxyCO2(isRealDataset ? real : test)}</p>

