<script lang='ts'>

	import { Day14Real, Day14Test } from './dataset';

	const real = Day14Real;
	const test = Day14Test;

	let isRealDataset = false;

	interface Rule {
		check: string;
		insert: string;
	}

	function applyPolymer(input: string, rules: Rule[]): string {

		const chars = input.split('');

		for (let i = 0; i < input.length; i++) {
			rules.forEach(r => {
					if (input.charAt(i) + input.charAt(i + 1) === r.check) {
						chars[i] = input.charAt(i) + r.insert;
					}
				}
			);
			input.charAt(i) + input.charAt(i + 1);
		}

		return chars.reduce((p, c) => `${p}${c}`);
	}

	function polymer(dataset: { input: string, rules: string }, steps: number): number {
		let input = dataset.input;
		let rules: Rule[] = dataset.rules.split('\n')
			.map(
				line => {
					const [check, _, insert] = line.split(' ');
					return { check, insert };
				}
			);

		for (let i = 0; i < steps; i++) {
			input = applyPolymer(input, rules);
		}

		const count = {};

		for (const element of input.split('')) {
			if (count[element]) {
				count[element] += 1;
			} else {
				count[element] = 1;
			}
		}

		const countArr: number[] = Object.entries(count).map(
			arr => +arr[1]
		);

		console.log('POLYMER', count);


		return Math.max(...countArr) - Math.min(...countArr);
	}


	function applyPolypair(pairs: Map<string, number>, rules: Rule[]): Map<string, number> {

		let newPairs = new Map<string, number>();

		pairs.forEach(
			(v, p) => {
				rules.forEach(r => {
						if (p === r.check) {
							const pair = r.check;
							const p1 = pair.split('')[0] + r.insert;
							const p2 = r.insert + pair.split('')[1];

							newPairs.set(p1, (newPairs.get(p1) ?? 0) + (pairs.get(pair) ?? 0 > 0 ? pairs.get(pair) : 0));
							newPairs.set(p2, (newPairs.get(p2) ?? 0) + (pairs.get(pair) ?? 0 > 0 ? pairs.get(pair) : 0));
							pairs.set(pair, 0);
						}
					}
				);
				if (pairs.get(p) !== 0) {
					newPairs.set(p, v);
				}
			}
		);
		return newPairs;
	}

	function polypairs(dataset: { input: string, rules: string }, steps: number): number {
		let rules: Rule[] = dataset.rules.split('\n')
			.map(
				line => {
					const [check, _, insert] = line.split(' ');
					return { check, insert };
				}
			);

		let pairs = new Map<string, number>();

		for (let i = 0; i < dataset.input.length - 1; i++) {
			const pair = dataset.input.charAt(i) + dataset.input.charAt(i + 1);
			if (pairs.get(pair)) {
				pairs.set(pair, pairs.get(pair)+1);
			} else {
				pairs.set(pair, 1);
			}

		}

		for (let i = 0; i < steps; i++) {
			pairs = applyPolypair(pairs, rules);
		}

		let count = new Map<string, number>();

		pairs.forEach((v, p) => {
				const [c1, c2] = p.split('');
				count.set(c1, (count.get(c1) ?? 0) + (pairs.get(p) ?? 0 > 0 ? pairs.get(p) : 0));
				count.set(c2, (count.get(c2) ?? 0) + (pairs.get(p) ?? 0 > 0 ? pairs.get(p) : 0));
			}
		);

		count.set(dataset.input.charAt(0), count.get(dataset.input.charAt(0))+1);
		count.set(dataset.input.charAt(dataset.input.length-1), count.get(dataset.input.charAt(dataset.input.length-1))+1);


		count.forEach(
			(value, key) =>
			count.set(key, value/2)
		);
		const countArr: number[] = Array.from(count.values());
		console.log('pairs', pairs, 'count', count);


		return Math.max(...countArr) - Math.min(...countArr);
	}


</script>

<h1>Day 14</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{polymer(isRealDataset ? real : test, 10)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{polypairs(isRealDataset ? real : test, 40)}</p>
