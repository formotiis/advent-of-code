<script lang='ts'>

	import { Day16Real, Day16Test } from './dataset';

	const real = Day16Real;
	const test = Day16Test;

	let isRealDataset = false;

	interface Packet {
		version: number;
		type: number;
		value?: number;
		subPackets?: Packet[];
	}

	function parseToBinary(dataset: string) {
		return dataset.split('').map(
			c => parseInt(c, 16).toString(2).padStart(4, '0')
		).reduce((p, c) => `${p}${c}`);
	}

	function getVersionNumber(input: string): number {
		const vn = input.slice(0, 3);
		console.log('vn', vn);
		return parseInt(vn, 2);
	}

	function getTypeID(input: string): number {
		const tid = input.slice(3, 6);
		console.log('tid', tid);
		return parseInt(tid, 2);
	}

	function getSumVersion(input: Packet): number {
		if (input.value !== undefined ) {
			return input.version;
		}
		return input.version + input.subPackets.reduce((sum, sub) => sum + getSumVersion(sub), 0);
	}

	function reduce(input: string[]): string {
		if (input.length === 0){
			return ''
		} else if (input.length === 1){
			return input[0];
		}
		return input.reduce((p,c) => `${p}${c}`)
	}

	function parsePacket(data: string): { packet: Packet; rest: string } {
		const chars = data.split('');
		const version = parseInt(reduce(chars.splice(0, 3)), 2);
		const type = parseInt(reduce(chars.splice(0, 3)), 2);

		if (type === 4) {
			return readConst(version, type, chars);
		}

		const lenghTypeID = chars.splice(0, 1)[0];

		if (lenghTypeID === '0') {
			const subPacketsLength = parseInt(reduce(chars.splice(0, 15)), 2);
			let subPacketContent = reduce(chars.splice(0, subPacketsLength));

			const subPackets: Packet[] = [];

			while (subPacketContent.length > 0) {
				const { packet, rest } = parsePacket(subPacketContent);

				subPackets.push(packet);
				subPacketContent = rest;
			}

			const packet: Packet = { version, type, subPackets };
			return { packet, rest: reduce(chars) };
		} else {
			const subPacketCount = parseInt(reduce(chars.splice(0, 11)), 2);
			let subData = reduce(chars);

			const subPackets: Packet[] = [];

			for (let i = 0; i < subPacketCount; i++) {
				const { packet, rest } = parsePacket(subData);

				subPackets.push(packet);
				subData = rest;
			}

			const packet: Packet = { version, type, subPackets };
			return { packet, rest: subData };
		}
	}

	function readConst(version, type, chars: string[]): { packet: Packet, rest: string } {
		let literalBinary = '';

		while (true) {
			const sub = reduce(chars.splice(0, 5)).padEnd(5, '0');
			literalBinary += sub.substring(1);

			if (sub[0] === '0') {
				break;
			}
		}

		const packet: Packet = { version, type, value: parseInt(literalBinary, 2) };
		return { packet, rest: reduce(chars) };
	}

	function valueOf(input: Packet): number {
		if (input.value !== undefined ) {
			return input.value;
		}

		const subPacketValues = input.subPackets.map(subPacket => valueOf(subPacket));

		switch (input.type) {
			case 0:
				return subPacketValues.reduce((a, b) => a + b);
			case 1:
				return subPacketValues.reduce((a, b) => a * b);
			case 2:
				return Math.min(...subPacketValues);
			case 3:
				return Math.max(...subPacketValues);
			case 5:
				return subPacketValues[0] > subPacketValues[1] ? 1 : 0;
			case 6:
				return subPacketValues[0] < subPacketValues[1] ? 1 : 0;
			case 7:
				return subPacketValues[0] === subPacketValues[1] ? 1 : 0;
			default:
				return -Infinity;
		}
	}

	function versions(dataset: string) {
		return getSumVersion(parsePacket(parseToBinary(dataset)).packet);
	}

	function compute(dataset: string) {
		return valueOf(parsePacket(parseToBinary(dataset)).packet);
	}


</script>


<h1>Day 16 </h1>
<label>
	<input type='checkbox' bind:value={isRealDataset}>
	Toggle Real Data
</label>
<p class='lead'> Results on First: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{versions(isRealDataset ? real : test)}</p>
<p class='lead'> Results on Second: {isRealDataset ? 'Real' : 'Test'} dataset</p>
<p>{compute(isRealDataset ? real : test)}</p>
