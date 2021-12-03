<script lang='ts'>
	import { Day02Real, Day02Test } from './dataset';

	const real = Day02Real;
	const test = Day02Test;

	let isRealDataset = false;

	export interface Instruction {
		direction: string;
		might: number;
	}

	function readInstruction(input: string): Instruction {
		const parts = input.split(' ');
		if (parts.length === 2) {
			return {
				direction: parts[0],
				might: +parts[1]
			}
		}
		return null;
	}

	function applyInstruction(inst: Instruction, depth: number, position: number): number[] {
		switch (inst.direction){
			case 'forward':
				position += inst.might;
				break;
				case 'down':
					depth += inst.might
				break;
				case 'up':
					depth -= inst.might;
				break;
				default:
				break;
		}
		return [depth, position];
	}
	function applyInstAim(inst: Instruction, depth: number, position: number, aim: number): number[] {
		switch (inst.direction){
			case 'forward':
				position += inst.might;
				depth += inst.might * aim;
				break;
				case 'down':
					aim += inst.might
				break;
				case 'up':
					aim -= inst.might;
				break;
				default:
				break;
		}
		return [depth, position, aim];
	}

	function calcTotal(dataset: string[]): number {
		let d = 0;
		let p = 0;
		dataset.map(line => readInstruction(line)).forEach(i => [d, p] = applyInstruction(i, d, p))
		return d*p;
	}

	function calcAim(dataset: string[]): number {
		let d = 0;
		let p = 0;
		let a = 0;
		dataset.map(line => readInstruction(line)).forEach(i => [d, p, a] = applyInstAim(i, d, p, a))
		return d*p;
	}

</script>

<h1>Day 02</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calcTotal(isRealDataset ? real : test)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calcAim(isRealDataset ? real : test)}</p>

