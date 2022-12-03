<script lang="ts">


	import { Day03Real, Day03Test } from './dataset';

	const real = Day03Real;
	const test = Day03Test;

	let isRealDataset = false;

	export interface Sack {
		first: string;
		second: string;
	}


	function read(dataset: string): Sack[] {
		return  dataset.split('\n')
			.map( sack => {
				const idx = sack.length / 2;
				return {first: sack.substring(0, idx),
				second: sack.substring(idx)}
			}
		)
	}

	function read2(dataset: string): string[][] {
		const res = [];
		const sacks = dataset.split('\n');
		for(let i = 0; i < sacks.length; i+=3) {
			res.push([sacks[i], sacks[i+1], sacks[i+2]])
		}
		return res;
	}

	function findUnique(a, b): string {
		for( let i = 0, len = b.length; i < len; i++ )
			if(b.indexOf(a[i]) != -1)
				return a[i];

		return "";
	}

	function findUnique2(a, b, c): string {
		if(b.length < c.length){
			return findUnique2(b, c, a)
		}
		if(b.length < a.length){
			return findUnique2(b, a, c)
		}

		for( let i = 0, len = b.length; i < len; i++ ) {
			if(b.indexOf(a[i]) != -1 && c.indexOf(a[i]) != -1){
				return a[i];
			}
		}

		return "";
	}

	function value(input: string){
		if (input.toUpperCase() === input){
			return (input.charCodeAt(0) - 'A'.charCodeAt(0)) + 27;

		}
		return (input.charCodeAt(0) - 'a'.charCodeAt(0)) + 1;
	}

	function calc(dataset: string): number {
		return read(dataset)
			.map(s => value(findUnique(s.first, s.second)))
		.reduce((a, c) => a + c);
	}

	function calc2(dataset: string): number {
		return read2(dataset)
			.map(s => value(findUnique2(s[0], s[1], s[2])))
			.reduce((a, c) => a + c);
	}

</script>


<h1>Day 02</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calc(isRealDataset ? real : test)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{calc2(isRealDataset ? real : test)}</p>

