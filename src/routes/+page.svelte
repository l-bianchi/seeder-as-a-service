<script lang="ts">
	import { Input, Button, Select } from 'flowbite-svelte';
	import { PlusOutline } from 'flowbite-svelte-icons';
	let raws: number = 1;
	let selected: any;
	let types = [
		{ value: 'string', name: 'String' },
		{ value: 'number', name: 'Number' }
	];
</script>

<div class="p-10 space-y-4">
	{#each [...Array(raws).keys()] as raw}
		<div class="flex w-full space-x-4">
			<Select class="w-fit" placeholder="Select a type" items={types} bind:value={selected} />
			<Input placeholder="Type field name" />
		</div>
	{/each}
	<Button on:click={() => raws++}>
		<PlusOutline class="w-5 h-5" />
	</Button>
</div>
