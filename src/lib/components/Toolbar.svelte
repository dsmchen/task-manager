<script lang="ts">
	import type { Filter } from '#lib/types.ts';

	let {
		activeCount = 0,
		doneCount = 0,
		filter,
		onFilterChange,
		onClear
	}: {
		activeCount?: number;
		doneCount?: number;
		filter: Filter;
		onFilterChange: (f: Filter) => void;
		onClear: () => void;
	} = $props();

	const FILTERS: Filter[] = ['all', 'active', 'done'];
</script>

<div class="stats">
	<p class="count" aria-live="polite">
		{activeCount} task{activeCount === 1 ? '' : 's'} left
	</p>
	<button type="button" class="clear" disabled={doneCount === 0} onclick={onClear}
		>clear done</button
	>
</div>

<div class="filters" role="group" aria-label="Filter tasks">
	{#each FILTERS as f (f)}
		<button
			type="button"
			class:active={filter === f}
			aria-pressed={filter === f}
			onclick={() => onFilterChange(f)}>{f}</button
		>
	{/each}
</div>

<style>
	.stats {
		display: flex;
	}
	.stats .count {
		margin: 0.5rem 0;
		font-weight: 600;
	}
	.stats .clear {
		margin-left: auto;
		color: #a33;
	}
	.stats .clear:disabled {
		opacity: 0.4;
		cursor: default;
	}
	.filters {
		display: flex;
		gap: 0.4rem;
		margin-bottom: 1rem;
	}
	.filters button {
		padding: 0.3rem 0.6rem;
		border: 1px solid #888; /* 3.54:1 — clears the 3:1 non-text bar */
		border-radius: 6px;
		background: none;
		cursor: pointer;
	}
	.filters button.active {
		background: #3b62f0; /* 5.01:1 with white text */
		color: white;
		border-color: #3b62f0;
	}
</style>
