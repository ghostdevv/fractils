<script lang="ts">
	import Example from '$examples/_lib/Item/Example.svelte'
	import Item from '$examples/_lib/Item/Item.svelte'
	import Params from '$examples/_lib/Item/Params.svelte'
	import { onMount } from 'svelte'

	let _clickOutside
	onMount(async () => {
		const { clickOutside } = await import('../../lib/actions/clickOutside')
		// @ts-ignore
		_clickOutside = clickOutside
	})

	let clickedOutside = false
	let timer: ReturnType<typeof setTimeout> | null = null
	const handleClickOutside = (e: CustomEvent) => {
		if (timer) clearTimeout(timer)
		clickedOutside = true
		timer = setTimeout(() => {
			clickedOutside = false
		}, 1000)
	}

	const example = `<script>
	// import { clickOutside } from 'fractils'

	let clickedOutside

	function handleClickOutside() {
		clickedOutside = true
		setTimeout(() => { clickedOutside = false }, 1000)
	}
<\/script>

<div use:clickOutside={handleClickOutside}>
	clickedOutside: {clickedOutside}
</div>
`

	const path = 'actions/clickOutside.ts'

	const params = [
		{
			type: 'param',
			title: 'callback',
			description: 'The function to call',
		},
	]
</script>

<Item title="clickOutside" type="action" {path}>
	<div slot="description">
		Calls a function when the user clicks outside the element.

		<Params {params} />
	</div>

	<Example example={example.replace('// ', '')} --h="382px">
		{#if _clickOutside}
			<div class="clickoutside" class:clickedOutside use:_clickOutside={handleClickOutside}>
				clickedOutside = {clickedOutside}
			</div>
		{/if}
	</Example>
</Item>

<style lang="scss">
	.clickoutside {
		display: flex;
		align-items: center;
		justify-content: center;

		width: 90%;
		height: 50px;
		margin: auto;

		color: var(--text);

		transition: color 0.2s ease;

		&:hover {
			color: #666666;
		}
	}
	.clickedOutside {
		color: var(--color-primary);
	}
</style>
