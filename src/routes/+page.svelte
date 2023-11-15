<script lang="ts">
	import { Input, Button, Select, Breadcrumb, BreadcrumbItem } from 'flowbite-svelte';
	import { PlusOutline } from 'flowbite-svelte-icons';

	const types: { value: string; name: string }[] = [
		{ value: 'string', name: 'string' },
		{ value: 'number', name: 'number' },
		{ value: 'boolean', name: 'boolean' },
		{ value: 'date', name: 'date' },
		{ value: 'object', name: 'object' },
		{ value: 'array', name: 'array' }
	];

	let current: string = 'root';
	let nodes: any = { root: { nav: 'root', raws: 1, types: [], inputs: [] } };

	function goDeep(newNode: string) {
		if (!nodes[newNode]) {
			nodes[newNode] = {
				nav: `${nodes[current].nav}/${newNode}`,
				raws: 1,
				types: [],
				inputs: []
			};
		}
		current = newNode;
	}
</script>

<div class="p-4">
	<Breadcrumb solid>
		{#each nodes[current].nav.split('/') as depth}
			<BreadcrumbItem home={depth === 'root'}>
				<Button
					color="none"
					class="p-0 bg-transparent text-lg text-bold focus:ring-0"
					on:click={() => (current = depth)}
				>
					{depth}
				</Button>
			</BreadcrumbItem>
		{/each}
	</Breadcrumb>
</div>
<div class="p-4 space-y-4">
	{#key current}
		{#each [...Array(nodes[current].raws).keys()] as _, index}
			<div class="flex w-full space-x-4">
				<Select
					class="w-fit uppercase"
					placeholder=""
					items={types}
					bind:value={nodes[current].types[index]}
				/>
				<Input placeholder="Type field name" bind:value={nodes[current].inputs[index]} />
				{#if nodes[current].types[index] === 'object'}
					<Button on:click={() => goDeep(nodes[current].inputs[index])}>open</Button>
				{/if}
			</div>
		{/each}
		<Button on:click={() => nodes[current].raws++}>
			<PlusOutline class="w-5 h-5" />
		</Button>
	{/key}
</div>
