<script lang='ts'>

	import { Day11Real, Day11Test } from './dataset';

	const real = Day11Real;
	const test = Day11Test;

	let isRealDataset = false;

	export interface Coordinates {
		x: number;
		y: number;
	}

	function datasetToMap(dataset: string): number[][] {
		return dataset.split('\n').map(
			(line, index) => line.split('').map(s => +s)
		);
	}

	function stepIncr(x: number, y: number, map: number[][], alreadyFlashed: Coordinates[]): void {
		if (!alreadyFlashed.find(obj => obj.x === x && obj.y === y)) {
			map[x][y] = map[x][y] + 1;

			if (map[x][y] > 9) {
				map[x][y] = 0;
				alreadyFlashed.push({ x, y });
				if (x > 0) {
					stepIncr(x - 1, y, map, alreadyFlashed);
				}
				if (x > 0 && y > 0) {
					stepIncr(x - 1, y - 1, map, alreadyFlashed);
				}
				if (y > 0) {
					stepIncr(x, y - 1, map, alreadyFlashed);
				}
				if (x < map.length -1 ) {
					stepIncr(x + 1, y, map, alreadyFlashed);
				}
				if (x < map.length - 1 && y < map[0].length - 1) {
					stepIncr(x + 1, y + 1, map, alreadyFlashed);
				}
				if (y < map[0].length - 1) {
					stepIncr(x, y + 1, map, alreadyFlashed);
				}
				if (x > 0 && y < map[0].length - 1) {
					stepIncr(x - 1, y + 1, map, alreadyFlashed);
				}
				if (y > 0 && x < map[0].length - 1) {
					stepIncr(x + 1, y - 1, map, alreadyFlashed);
				}
			}
		}
	}

	function calcFlashes(dataset: string, steps: number): number {
		const map = datasetToMap(dataset);
		let totalFlash = 0;

		for (let i = 0; i < steps; i++) {
			const flashed = [];

			for (let x = 0; x < map.length; x++) {
				for (let y = 0; y < map.length; y++) {
					stepIncr(x, y, map, flashed)
				}
			}

			totalFlash += flashed.length;
		}

		return totalFlash;
	}

	function calcFirstStep(dataset: string): number {
		const map = datasetToMap(dataset);

		let totalFlashes = [];
		let step = 0;
		while (totalFlashes.length === 0) {
			const flashed = [];

			for (let x = 0; x < map.length; x++) {
				for (let y = 0; y < map.length; y++) {
					stepIncr(x, y, map, flashed)
				}
			}

			if (map.map(line => line.reduce((p,c) => p+c)).reduce((p,c) => p+c) === 0) {
				totalFlashes.push(step+1);
				return step +1;
			}
			step++;
		}

		return totalFlashes.length > 0 ? totalFlashes[0] : -1	;
	}

</script>

<h1>Day 11</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calcFlashes(isRealDataset ? real : test, 100)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calcFirstStep(isRealDataset ? real : test)}</p>
