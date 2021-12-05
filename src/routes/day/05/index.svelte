<script lang="ts">


	import { Day05Real, Day05Test } from './dataset';

	const real = Day05Real;
	const test = Day05Test;

	let isRealDataset = false

	export interface Coordinates{
		x: number;
		y: number;
	}

	export interface Store{
		n: number;
		c: Coordinates;
	}

	export interface Vector {
		orig: Coordinates;
		dest: Coordinates;
	}

	function equals(c1: Coordinates, c2: Coordinates): boolean {
		return c1.x === c2.x && c1.y === c2.y
	}

	function stringToCoords(input: string): Coordinates {
		const split =  input.split(',');
		return {x: +split[0], y: +split[1]};
	}

	function readDataset(dataset: string[]): Vector[] {
		return dataset.map(
			line => {
				const vect = line.split(' ');
				return {orig: stringToCoords(vect[0]), dest: stringToCoords(vect[2])}
			}
		);
	}

	function isVectorVertical(v: Vector): boolean {
		return v.orig.x === v.dest.x
	}

	function isVectorHorizontal(v: Vector): boolean {
		return v.orig.y === v.dest.y
	}

	function isVectorALine(v: Vector): boolean {
		return isVectorVertical(v) || isVectorHorizontal(v);
	}

	function storeCoordinates(c: Coordinates, store: Store[]): void {
		const entry = store.find(se => equals(se.c,c));
		if (entry) {
			entry.n += 1;
		} else {
			store.push( {c: c, n: 1});
		}
	}

	function findIncr(org: number, dst: number): number {
		return org < dst ? 1 : -1;
	}

	function addPointsOfVector(v: Vector, store: Store[], inclDiag: boolean): void {
		if (isVectorHorizontal(v)){
			for (let x = Math.min(v.orig.x, v.dest.x); x <= Math.max(v.dest.x, v.orig.x); x++) {
				storeCoordinates({x: x, y: v.orig.y}, store)
			}
		} else if (isVectorVertical(v)){
			for (let y = Math.min(v.orig.y, v.dest.y); y <= Math.max(v.orig.y, v.dest.y); y++) {
				storeCoordinates({x: v.orig.x, y: y}, store)
			}
		} else if (inclDiag) {
			let x = v.orig.x;
			let y = v.orig.y;
			storeCoordinates({x: x, y: y}, store);
			while(x !== v.dest.x && y !== v.dest.y) {
				x = x + findIncr( v.orig.x, v.dest.x);
				y = y + findIncr( v.orig.y, v.dest.y);
				storeCoordinates({x: x, y: y}, store);
			}
		}
	}

	function calcPoints(dataset: string[], inclDiag: boolean): number {
		const vectors = readDataset(dataset);
		const store: Store[] = [];

			vectors.forEach(
				vect => addPointsOfVector(vect, store, inclDiag)
			);

		if (inclDiag) {
			console.log(store);
		}
		return store.filter(se => se.n > 1).length;
	}


</script>

<h1>Day 05</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calcPoints(isRealDataset ? real : test, false)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calcPoints(isRealDataset ? real : test, true)}</p>


