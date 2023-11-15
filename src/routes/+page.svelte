<script lang="ts">
	import {
		Input,
		GradientButton,
		Button,
		Badge,
		Breadcrumb,
		BreadcrumbItem,
		Modal
	} from 'flowbite-svelte';
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
	let nodes: any = { root: { path: ['root'], list: [] } };
	let showModal = false;

	function addRaw(type: string) {
		nodes[current].list = [...nodes[current].list, { type, value: '' }];
		showModal = false;
	}

	function goDeeper(newNode: string) {
		if (!nodes[newNode]) {
			nodes[newNode] = {
				path: [...nodes[current].path, newNode],
				list: []
			};
		}
		current = newNode;
	}
</script>

<div class="w-screen h-screen dark:bg-gray-900">
	<div class="p-4">
		<Breadcrumb solid>
			{#each nodes[current].path as path}
				<BreadcrumbItem home={path === 'root'}>
					<Button
						color="none"
						class="p-0 bg-transparent text-lg text-bold focus:ring-0"
						on:click={() => (current = path)}
					>
						{path}
					</Button>
				</BreadcrumbItem>
			{/each}
		</Breadcrumb>
	</div>
	<div class="flex flex-col p-4 space-y-4">
		{#key current}
			{#each nodes[current].list as { type, value, color }}
				<div class="flex w-full space-x-4">
					<Badge border color="purple">{type}</Badge>
					<Input placeholder="Type field name" bind:value />
					{#if type === 'object'}
						<GradientButton outline shadow color="purpleToPink" on:click={() => goDeeper(value)}>
							<p class="text-bold uppercase">open</p>
						</GradientButton>
					{/if}
				</div>
			{/each}
		{/key}
		<GradientButton
			outline
			shadow
			color="purpleToPink"
			class="w-fit self-end"
			on:click={() => (showModal = true)}
		>
			<PlusOutline class="w-5 h-5 outline-none" />
		</GradientButton>
	</div>
</div>

<Modal title="Select a type" bind:open={showModal} autoclose outsideclose>
	<div class="grid grid-cols-3 gap-4">
		{#each types as { value, name }}
			<GradientButton outline shadow color="purpleToPink" on:click={() => addRaw(value)}>
				<p class="text-lg text-bold uppercase">{name}</p>
			</GradientButton>
		{/each}
	</div>
</Modal>
