<script lang='ts'>
	import { Day02Real, Day02Test } from './dataset';

	const real = Day02Real;
	const test = Day02Test;

	let isRealDataset = false;

	export interface Round {
		baddie: string;
		player: string;
	}


	function read(input: string): Round[] {
		return input.split('\n').map(r => {
				const split = r.split(' ');
				return { baddie: split[0], player: split[1] };
			}
		);
	}

	function battleScore(round: Round): number {
		switch (round.player) {
			case 'X':
				return 1 + (round.baddie === 'B' ? 0 : (round.baddie === 'C' ? 6 : 3));
			case 'Y':
				return 2 + (round.baddie === 'C' ? 0 : (round.baddie === 'A' ? 6 : 3));
			case 'Z':
				return 3 + (round.baddie === 'A' ? 0 : (round.baddie === 'B' ? 6 : 3));
				default:
					return 0;
		}
	}

	function battleScore2(round: Round): number {
		switch (round.baddie) {
			case 'A':
				return (round.player === 'X' ? 3 : (round.player === 'Z' ? 8 : 4))
			case 'B':
				return (round.player === 'X' ? 1 : (round.player === 'Z' ? 9 : 5))
			case 'C':
				return (round.player === 'X' ? 2 : (round.player === 'Z' ? 7 : 6))
			default:
				return 0;
		}
	}

	function calc1(input: string): number {
		return read(input).map(r => battleScore(r)).reduce((a, c) => a+c);
	}
	function calc2(input: string): number {
		return read(input).map(r => battleScore2(r)).reduce((a, c) => a+c);
	}


</script>

<h1>Day 02</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calc1(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{calc2(isRealDataset ? real : test)}</p>

