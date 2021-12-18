<script lang="ts">

	import { Day17Real, Day17Test } from './dataset';

	const real = Day17Real;
	const test = Day17Test;

	let isRealDataset = false;

	export interface Area{
		min: number;
		max: number;
	}

	export interface Position{
		x: number;
		y: number;
	}

	export interface TargetArea {
		x: Area;
		y: Area;
	}

	function readDataset(dataset: string): TargetArea {
		const [t, tx, y] = dataset.split('=');
		const x = tx.split(',')[0];

		return {
			x: {min: parseInt(x.split('..')[0]), max: parseInt(x.split('..')[1])},
			y: {min: parseInt(y.split('..')[0]), max: parseInt(y.split('..')[1])},
		};
	}

	function step(c: Position, v: Position): [Position, Position] {
		return [{x: c.x + v.x, y: c.y + c.y}, {x: v.x === 0 ? 0: (v.x > 0 ? v.x -1: v.x + 1), y: v.y -1.}];
	}

	function checkHit(v: Position, target: TargetArea): true | Position {

		let velocity = {x: v.x, y: v.y};
		let position = {x: 0, y: 0};

		while (position.x <= target.x.max && position.y >= target.y.max) {
			if (position.x <= target.x.max && position.y >= target.y.max
				&& position.x >= target.x.min && position.y <= target.y.min){
				return true
			}
			[position, velocity] = step(position, velocity);
		}

		return {x: position.x > target.x.max ? 0 : 1, y: position.y < target.y.max ? 1 : 0};

	}

	function posFindr(dataset: string): number {
		const target = readDataset(dataset);
		let topY = 0;
		let testVelocity = {x: 0, y: 0};
		let attempts = 0;

		goto: while (true) {
			let position = {x: 0, y: 0};
			let velocity = {x: testVelocity.x, y: testVelocity.y};
			let topLocalY = 0;

			while (true) {
				const lastPosition = {x: position.x, y: position.y };
				position = {x: position.x + velocity.x, y: position.y + velocity.y};
				if (position.y > topLocalY) {
					topLocalY = position.y;
				}

				velocity = {x: velocity.x - Math.sign(velocity.x), y: velocity.y - 1}

				if ( position.x >= target.x.min && position.x <= target.x.max &&
					position.y >= target.y.min && position.y <= target.y.max) {
					attempts = 0;
					topY = topLocalY;
					testVelocity = {x: testVelocity.x, y: testVelocity.y + 1 };
					break;
				}

				if (position.y < target.y.min) {
					if (lastPosition.y > target.y.max) {
						attempts++;
						if (attempts > 100){
							break goto;
						}
						testVelocity = {x: testVelocity.x, y: testVelocity.y +1};
						break;
					}

					if (lastPosition.x < target.x.min) {
						testVelocity = {x: testVelocity.x +1, y: testVelocity.y};
					} else if (lastPosition.x > target.x.max) {
						testVelocity = {x: testVelocity.x - 1, y: testVelocity.y};
					}
					break;
				}
			}
		}

		return topY;
	}

	function allFindr(dataset: string): number {
		const target = readDataset(dataset);
			let hits = 0;
			let minX = Math.floor((-1 + Math.sqrt(1 - 4 * 1 * (-2 * target.x.min)) / 2));
			for (let x = minX; x <= target.x.max; x++) {
				for (let y = target.y.min; y <= Math.abs(target.y.min); y++) {
					if (hitsTarget({x, y}, target)) {
						hits++;
					}
				}
			}
			return hits
	}

	function hitsTarget(velocity: Position, target: TargetArea): boolean {
		let probe = {x: 0, y: 0, velocity};
		if (velocity.y > 0) {
			let newY = -(probe.velocity.y+1);
			let YtoZero = velocity.y*2+1
			probe.velocity.y = newY;
			let x = probe.velocity.x*(probe.velocity.x+1)/2;
			probe.velocity.x = Math.max(0, velocity.x - YtoZero);
			let xWithVelocity = probe.velocity.x*(probe.velocity.x+1)/2;
			probe.x = x - xWithVelocity;
		}

		while (probe.x < target.x.max && probe.y >= target.y.min) {
			probe.x += probe.velocity.x;
			probe.y += probe.velocity.y;
			probe.velocity.x -= Math.sign(probe.velocity.x);
			probe.velocity.y--;
			if (probe.x >= target.x.min && probe.x <= target.x.max && probe.y <= target.y.max && probe.y >= target.y.min) {
				return true;
			}
		}
		return false;
	}

</script>


<h1>Day 17</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{posFindr(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{allFindr(isRealDataset ? real : test)}</p>

