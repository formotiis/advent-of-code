<script lang='ts'>

	import { Day15Real, Day15Test } from './dataset';
	import Heap from 'heap-js';

	const real = Day15Real;
	const test = Day15Test;

	let isRealDataset = true;

	function npp(n: number): number{
		return n > 9 ? npp(n-9) : n;
	}

	function datasetToMap(dataset: string) : number[][] {
		return dataset.split('\n').map(
			line => line.split('').map(n => +n)
		);
	}

	function datasetToBiggerMap(dataset: string) : number[][] {

		const map1 = dataset.split('\n').map(
			line => line.split('').map(n => +n)
		).map(
			line => [...line, ...line.map(n => npp(n+1)),
				...line.map(n => npp(n+2)), ...line.map(n => npp(n+3)), ...line.map(n => npp(n+4))]
		)
		const map2 = map1.map(line => line.map (n => npp(n+1)));
		const map3 = map2.map(line => line.map (n => npp(n+1)));
		const map4 = map3.map(line => line.map (n => npp(n+1)));
		const map5 = map4.map(line => line.map (n => npp(n+1)));

		return [...map1, ...map2, ...map3, ...map4, ...map5];
	}

	function heapOrder(a: { x: number; y: number; weight: number },
											b: { x: number; y: number; weight: number }): number {
		return a.weight - b.weight;
	}

	function findPath(map: number[][]): number {
		const heap = new Heap(heapOrder);

		const visited = new Set<string>();
		heap.push({ x: 0, y: 0, weight: 0 });
		visited.add("0,0");

		while (heap.length > 0) {
			const current = heap.pop();
			if (current.x == map[0].length - 1 && current.y == map.length - 1) {
				return current.weight;
			}

			if (current.x > 0) {
				if(!visited.has(`${current.x-1},${current.y}`)){
					visited.add(`${current.x-1},${current.y}`);
					heap.push({ x: current.x-1, y: current.y, weight: current.weight + map[current.x-1][current.y]});
				}
			}
			if (current.x < map.length - 1) {
				if(!visited.has(`${current.x+1},${current.y}`)){
					visited.add(`${current.x+1},${current.y}`);
					heap.push({ x: current.x+1, y: current.y, weight: current.weight + map[current.x+1][current.y]});
				}
			}
			if (current.y > 0) {
				if(!visited.has(`${current.x},${current.y-1}`)){
					visited.add(`${current.x},${current.y-1}`);
					heap.push({ x: current.x, y: current.y-1, weight: current.weight + map[current.x][current.y-1]});
				}
			}
			if (current.y < map[current.x].length - 1) {
				if(!visited.has(`${current.x},${current.y+1}`)){
					visited.add(`${current.x},${current.y+1}`);
					heap.push({ x: current.x, y: current.y+1, weight: current.weight + map[current.x][current.y+1]});
				}
			}
		}
	}

</script>

<h1>Day 15</h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{findPath(datasetToMap(isRealDataset ? real : test))}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{findPath(datasetToBiggerMap(isRealDataset ? real : test))}</p>
