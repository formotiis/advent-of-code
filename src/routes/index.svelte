<script lang='ts'>

	const years = [2021, 2022];
	const days: string[] = Array.from({length: 25}, (item, index) => ('00'+ `${index+1}`)
		.substring(`${index}`.length,`${index}`.length+2));

	let year = 2022;
	let day = '01';

</script>

<h1>Advent of Code</h1>

Year
<select bind:value={year}>
	{#each years as y}
		<option value={y}>
			{y}
		</option>
	{/each}
</select>

Day
<select bind:value={day}>
	{#each days as d}
		<option value={d}>
			{d}
		</option>
	{/each}
</select>


<p>Go to {year} day <a href='/{year}/{day}'>{day}</a></p>

