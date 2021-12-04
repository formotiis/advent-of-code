<script lang="ts">
	import { Day04Real, Day04Test } from './dataset';

	const real = Day04Real;
	const test = Day04Test;

	let isRealDataset = false;

	interface BingoTile {
		value: number;
		marked: boolean;
	}

	function convertBoards(input: string): BingoTile[][][] {
		const rawLines = input.split('\n');
		const array = rawLines.filter(l => !!l)
		const boards = [];
		const lines =  array.map( l =>
			l.split(' ')
				.map(s => ({value:+s, marked: false})));
		while (lines.length > 0) {
			boards.push(lines.splice(0,5))
		}
		return boards;
	}

	function markBoard(input: number, boards: BingoTile[][][]) {
		boards.forEach( board =>
		{
			board.find(
				line => line.find(tile => (tile.value === input) && markTile(tile))
			)
		});
	}

	function markTile(tile: BingoTile): boolean {
		tile.marked = true;
		return tile.marked;
	}

	function checkForBingo(board: BingoTile[][]): boolean {
		return checkRows(board) || chekColumns(board);
	}

	function checkRows(board: BingoTile[][]): boolean {
		return board.map( x =>
			x.map(y => y.marked)).map( row => row.reduce((p,c) => p && c)).some(row => row)
	}
	function chekColumns(board: BingoTile[][]): boolean {
		for (let i = 0; i < board[0].length; i++) {
			const col = board.map(d => d[i].marked);
			if (col.reduce((p,c) => p && c)) {
				return true
			}
		}
		return false;
	}

	function checkDataset(input: {boards: string, input: number[]}): number {
		const boards = convertBoards(input.boards);
		for (let i = 0; i < input.input.length; i++) {
			markBoard(input.input[i], boards);
			if (boards.some(board => checkForBingo(board))) {
				const winner = boards.find(board => checkForBingo(board));
				return input.input[i] * (winner.map(b => b.filter(_ => !_.marked).map(_ => _.value))
					.filter(s => s.length > 0)
					.map( s => s.reduce((p,c) => p+c)))
					.reduce((p,c) => p+c);
			}
		}
		return -1;
	}

	function checkLosingDataset(input: {boards: string, input: number[]}): number {
		const boards = convertBoards(input.boards);
		for (let i = 0; i < input.input.length; i++) {
			markBoard(input.input[i], boards);
			if (boards.some(board => checkForBingo(board))) {
				const winners = boards.filter(board => checkForBingo(board));
				if(boards.length > 1 || i === input.input.length -1) {
					winners.forEach( board =>
						boards.splice(boards.indexOf(board), 1)
					)
				} else {
					return input.input[i] * (boards.find(board => checkForBingo(board))
						.map(b => b.filter(_ => !_.marked).map(_ => _.value))
						.filter(s => s.length > 0)
						.map( s => s.reduce((p,c) => p+c)))
						.reduce((p,c) => p+c);
				}
			}
		}

		return -1;
	}

</script>

<h1>Day 04</h1>
<label>
	<input type=checkbox bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class="lead"> Results on First: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{checkDataset(isRealDataset ? real : test)}</p>
<p class="lead"> Results on Second: {isRealDataset? 'Real': 'Test'} dataset</p>
<p>{checkLosingDataset(isRealDataset ? real : test)}</p>
