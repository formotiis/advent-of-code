<script lang='ts'>

	import { Day13Real, Day13Test } from './dataset';

	const real = Day13Real;
	const test = Day13Test;

	let isRealDataset = false;

	type FoldDirection = string;

	export interface Coordinates{
		x: number;
		y: number;
	}

	export interface Instruction{
		direction: string;
		amp: number;
	}

	function stringToCoordinate(input: string): Coordinates[] {
		return input.split('\n').map(
			line => {
				const sides = line.split(',');
				return {x: +sides[0], y: +sides[1]}
			}
		)
	}

	function setToMap(input: Coordinates[]): string {
		let maxX = 0;
		let maxY = 0;

		console.log(input)
		input.forEach(
			c => {
				maxX = maxX > c.x ? maxX : c.x;
				maxY = maxY > c.y ? maxY : c.y;
			}
		);

		let map : string[][];

		map = []

		for (let x = 0; x < maxY+1; x++) {
			map [x] = [];
		}


			for (let x = 0; x < maxY+1; x++) {
				for (let y = 0; y < maxX+1; y++) {
				map[x].push(' ');
			}
		}

		input.forEach(
			c => {
				map[c.y][c.x] = '█'
			}
		)

		return map.map(line => line.reduce((p,c) => `${p}${c}`)).reduce((p,c) => `${p}<br/>${c}`)

	}

	function stringToInstruction(input: string): Instruction[] {
		return input.split('\n').map(
			line => {
				const sides = line.split(' ')[2].split('=');
				return {direction: sides[0], amp: +sides[1]}
			}
		)
	}

	function foldAlong(map: Coordinates[], foldDirection: FoldDirection, foldAmp: number): Coordinates[] {
		if( foldDirection === 'x') {
			return map.map(c =>
			{
				if (c.x > foldAmp){
					return {x: foldAmp - (c.x - foldAmp), y: c.y}
				} else {
					return c;
				}
			});
		} else {
			return map.map(c =>
			{
				if (c.y > foldAmp){
					return { x: c.x, y: foldAmp - (c.y - foldAmp)}
				} else {
					return c;
				}
			});
		}
	}

	function foldingFirst(dataset: {map: string, instructions:string}): number {
		let map = stringToCoordinate(dataset.map);
		const instructions = stringToInstruction(dataset.instructions);
		map = foldAlong(map, instructions[0].direction, instructions[0].amp);
		return new Set<string>(map.map(c => `${c.x},${c.y}`)).size;
	}

	function foldAll(dataset: {map: string, instructions:string}): string {
		let map = stringToCoordinate(dataset.map);
		const instructions = stringToInstruction(dataset.instructions);

		for (let i = 0; i < instructions.length; i++) {
			map = foldAlong(map, instructions[i].direction, instructions[i].amp);
		}

		return setToMap(map);
	}



</script>


<h1>Day 13</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{foldingFirst(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p style='font-family: monospace'>{@html foldAll(isRealDataset ? real : test)}</p>
