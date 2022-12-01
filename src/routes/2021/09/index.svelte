<script lang='ts'>

	import { Day09Real, Day09Test } from './dataset';

	const real = Day09Real;
	const test = Day09Test;

	let isRealDataset = false;

	enum Direction {
		UP = 'up', DOWN = 'down', LEFT =  'left', RIGHT = 'right'
	}

	function datasetToMap(dataset: string): number[][] {
		return dataset.split('\n').map(
			(line, index) => line.split('').map(s => +s)
		);
	}

	function isLowerThanNeighbours(x: number, y: number, map: number[][]) {
		let lower = true;

		if (x > 0) {
			lower = (map[x - 1][y] > map[x][y]);
		}
		if (x < map.length - 1) {
			lower = lower && (map[x + 1][y] > map[x][y]);
		}
		if (y > 0) {
			lower = lower && (map[x][y - 1] > map[x][y]);
		}
		if (y < map[x].length - 1) {
			lower = lower && (map[x][y + 1] > map[x][y]);
		}

		return lower;
	}

	function isLowerThanNeighboursBassin(x: number, y: number, map: number[][]) {
		let lower = true;

		if (x > 0) {
			lower = (map[x - 1][y] > map[x][y]);
		}
		if (x < map.length - 1) {
			lower = lower && (map[x + 1][y] > map[x][y]);
		}
		if (y > 0) {
			lower = lower && (map[x][y - 1] > map[x][y]);
		}
		if (y < map[x].length - 1) {
			lower = lower && (map[x][y + 1] > map[x][y]);
		}
		let v = -1;

		if (lower) {
			const marked = map.map( (l) => l.map(_ => false ));
			v = 1 +
				checkBassin((x-1), y, map[x][y], map, marked, Direction.DOWN) +
				checkBassin((x+1), y, map[x][y], map, marked, Direction.UP) +
				checkBassin(x, (y-1), map[x][y], map, marked, Direction.RIGHT) +
				checkBassin(x, (y+1), map[x][y], map, marked, Direction.LEFT);

		}

		return v;
	}

	function checkBassin(x: number, y: number, value: number, map: number[][], marks: boolean[][], origin: Direction): number {
		if (x < 0 || x > map.length - 1 || y < 0 || y > map[x].length - 1) {
			return 0;
		}
		if (map[x][y] < (value) || marks[x][y] || map[x][y] === 9) {
			return 0;
		}

		marks[x][y] = true;
		const v = (origin !== Direction.UP ? checkBassin(x - 1, y, map[x][y], map, marks, Direction.DOWN) : 0) +
			(origin !== Direction.DOWN ? checkBassin(x + 1, y, map[x][y], map, marks, Direction.UP) : 0) +
			(origin !== Direction.LEFT ? checkBassin(x, y - 1, map[x][y], map, marks, Direction.RIGHT) : 0) +
			(origin !== Direction.RIGHT ? checkBassin(x, y + 1, map[x][y], map, marks, Direction.LEFT) : 0);
		return 1 + v;
	}

	function calcRisk(dataset: string): number {
		const map = datasetToMap(dataset);

		const values = [];

		map.forEach(
			(line, x) => line.forEach(
				(value, y) => {
					if (isLowerThanNeighbours(x, y, map)) {
						values.push(value + 1);
					}
				}
			)
		);

		return values.reduce((p, c) => p + c);
	}

	function calcRiskBassin(dataset: string): number {
		const map = datasetToMap(dataset);

		const values = map.map(
			(line, x) => line.map(
				(value, y) => isLowerThanNeighboursBassin(x, y, map)
			)
		).flat().sort((a, b) => b-a);

		return values[0] * values[1] * values [2];
	}
</script>


<h1>Day 09</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calcRisk(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calcRiskBassin(isRealDataset ? real : test)}</p>
