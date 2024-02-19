<script lang="ts">
	import Icon from '@iconify/svelte';
	import LineParseTextrea from '../components/line-parse-textrea.svelte';

	let cubeList = $state<string[]>([]);
	let removedCards = $state<string[]>([]);

	let remainingCards = $derived.by(() => {
		const cubeSet = new Set(cubeList);
		removedCards.forEach(card => cubeSet.delete(card));
		return Array.from(cubeSet);
	});
</script>

<main>
	<section>
		<section class="header">
			<h2>Cube List</h2>

			<p class="count">{cubeList.length} cards</p>
		</section>

		<LineParseTextrea onLinesParsed={lines => (cubeList = lines)} />
	</section>

	<section>
		<section class="header">
			<h2>Removed Cards</h2>

			<p class="count">{removedCards.length} cards</p>
		</section>

		<LineParseTextrea onLinesParsed={lines => (removedCards = lines)} />
	</section>

	<section>
		<section class="header">
			<h2>Remaining Cards</h2>

			<Icon icon="material-symbols:content-copy-outline" width={30} />

			<p class="count">{remainingCards.length} cards</p>
		</section>

		<section class="remaining">
			{#each remainingCards as card}
				<div>{card}</div>
			{/each}
		</section>
	</section>
</main>

<style lang="scss">
	main {
		display: grid;
		gap: 2rem;
		grid-template-columns: repeat(3, 1fr);
		padding: 2rem;

		.header {
			align-items: center;
			display: flex;
			gap: 0.5rem;

			.count {
				margin-left: auto;
			}

			:global(svg) {
				cursor: pointer;
			}
		}

		.remaining {
			height: 45rem;
			overflow: auto;
		}
	}
</style>
