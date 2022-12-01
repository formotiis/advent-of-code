<script lang='ts'>

	import { Day06Real, Day06Test } from './dataset';

	const real = Day06Real;
	const test = Day06Test;

	let isRealDataset = false;


	function dayPassed(bay: number[]): number[] {
		let births = 0;
		const newBay = bay.map( fish => {
			if (fish -1 === -1) {
				births++;
				return 6;
			}
			return fish - 1;
		})

		for (let i = 0; i < births; i++) {
			newBay.push(8);
		}

		return newBay;
	}

	function evolve(dataset: number[], days: number): number {
		let bay = dataset.slice();
		for (let i = 0; i < days; i++) {
			bay = dayPassed(bay);
		}

		return bay.length;
	}
	function convert(dataset: number[]){
		return [dataset.filter(fish => fish === 0).length,
			dataset.filter(fish => fish === 1).length,
			dataset.filter(fish => fish === 2).length,
			dataset.filter(fish => fish === 3).length,
			dataset.filter(fish => fish === 4).length,
			dataset.filter(fish => fish === 5).length,
			dataset.filter(fish => fish === 6).length,
			dataset.filter(fish => fish === 7).length,
			dataset.filter(fish => fish === 8).length,
		];
	}

	function bayPassed(bay: number[]) {
		return bay.map(
			(p, i) => {
				if(i === 6) {
					return (bay[i + 1] + bay[0]);
				} else if (i === 8) {
					return bay[0]
				} else {
					return bay[i + 1]
				}
			}
		);
	}

	function evolveBays(dataset: number[], days: number): number {
		let bay = convert(dataset);
		for (let i = 0; i < days; i++) {
			bay = bayPassed(bay);
		}

		return bay.reduce((p,c) => p+c);
	}

</script>

<h1>Day 06</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{evolve(isRealDataset ? real : test, 80)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{evolveBays(isRealDataset ? real : test, 256)}</p>
