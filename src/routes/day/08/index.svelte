<script lang='ts'>


	import { Day08Real, Day08Test } from './dataset';

	const real = Day08Real;
	const test = Day08Test;

	let isRealDataset = false;

	interface Entry {
		pattern: number[];
		output: number[];
	}

	function readDataset(dataset: string): Entry[] {
		const lines = dataset.split('\n');

		return lines.map(line => {
			const entry = line.split(' | ');
			return {
				pattern: entry[0].split(' ').map((code, i, a) => deduceCode(code, a)),
				output: entry[1].split(' ').map(code => deduceCode(code, entry[0].split(' ')))
			};
		});
	}

	function deduceCode(entry: string, entries: string[]) {
		const four = entries.find(e => e.length === 4);
		const one = entries.find(e => e.length === 2);

		switch (entry.length) {
			case 2:
				return 1;
			case 3:
				return 7;
			case 4:
				return 4;
			case 7:
				return 8;
			case 6:
				return codeMatch(entry, four) ? 9 :
					codeMatch(entry, one) ? 0 : 6;
			case 5:
				return codeMatch(entry, one) ? 3 :
					codeMatchAtLeastThree(entry, four) ? 5 : 2;
			default:
				return -1;
		}
	}

	function codeMatch(a: string, b: string): boolean {
		if (a === b) {
			return true;
		}
		const chara = a.split('').sort(compare).reduce((p, c) => `${p}${c}`);
		const charb = b.split('').sort(compare);
		return charb.map(c => chara.includes(c)).reduce((p, c) => p && c);
	}

	function compare(a,b) {
		if (a < b)
			return -1;
		if (a > b)
			return 1;
		return 0;
	}


	function codeMatchAtLeastThree(a: string, b: string): boolean {
		if (a === b) {
			return true;
		}
		const chara = a.split('').sort();
		const charb = b.split('').sort();
		let cpt = 0;
		chara.forEach(
			char => {
				if (charb.includes(char)) {
					cpt++;
				}
			}
		);
		return cpt > 2;
	}

	function deduceNumbers(dataset: string): number {
		const entries: Entry[] = readDataset(dataset);
		let t = 0;
		entries.forEach(
			entry => {
				t += entry.output
					.filter(number => number === 1 || number === 4 || number === 7 || number === 8).length;
			}
		);
		return t;
	}

	function deduceNumbersComplex(dataset: string): number {
		const entries: Entry[] = readDataset(dataset);
		let t = 0;
		entries.forEach(
			entry => {
				t += +entry.output.map(out => '' + out).reduce((p, c) => `${p}${c}`);
			}
		);
		return t;
	}


</script>

<h1>Day 08</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{deduceNumbers(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{deduceNumbersComplex(isRealDataset ? real : test)}</p>

