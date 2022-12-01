<script lang='ts'>

	import { Day10Real, Day10Test } from './dataset';

	const real = Day10Real;
	const test = Day10Test;

	let isRealDataset = false;

	const corrupt = new Map<string, number>(
		[
			[')', 3],
			[']', 57],
			['}', 1197],
			['>', 25137],
		]
	);

	const complete = new Map<string, number>(
		[
			[')', 1],
			[']', 2],
			['}', 3],
			['>', 4],
		]
	);

	const counterparts = new Map<string, string>(
		[
			[')', '('],
			[']', '['],
			['}', '{'],
			['>', '<'],

			['(', ')'],
			['[', ']'],
			['{', '}'],
			['<', '>'],
		]
	);

	function isClosingBracket(input: string): boolean {
		return [')', ']', '}', '>'].includes(input);
	}

	function readStack(line: string): string {
		const chars = line.split('');

		let stack = '';
		let bc = '';

		chars.every(
			c => {
				if (isClosingBracket(c)) {
					if (counterparts.get(c) === stack.charAt(stack.length-1)) {
						stack = stack.slice(0, stack.length-1);
					} else {
						bc = c;
						return false
					}
				} else {
					stack += c;
				}
				return true;
			}
		);

		return bc;
	}

	function completeStack(line: string): string {
		const chars = line.split('');

		let stack = '';
		let suppChars = '';

		chars.every(
			c => {
				if (isClosingBracket(c)) {
					if (counterparts.get(c) === stack.charAt(stack.length-1)) {
						stack = stack.slice(0, stack.length-1);
					}
				} else {
					stack += c;
				}
				return true;
			}
		);

		stack.split('').forEach(
			c => suppChars = counterparts.get(c) + suppChars)

		return suppChars;
	}

	function calcErrors(dataset: string): number {
		let badChars = [];
		dataset.split('\n').forEach(
			line => badChars = [...badChars, readStack(line)]
		);

		return badChars.filter(_ => !!_).map(bad => corrupt.get(bad)).reduce((p, c) => p + c);
	}

	function completeValue(supp: string) {
		let v = 0;
		supp.split('').forEach(
			c => v = (5 * v) + complete.get(c)
		)
		return v;
	}

	function calcComplete(dataset: string): number {
		const lines = dataset.split('\n').filter(
			line => readStack(line) === ''
		);

		let supp = [];
		lines.forEach(
			line => supp = [...supp, completeStack(line)]
		);

		const scores = supp.map( cc => completeValue(cc)).sort((a,b) => b-a);

		return scores[(scores.length-1)/2];
	}

</script>


<h1>Day 10</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calcErrors(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calcComplete(isRealDataset ? real : test)}</p>
