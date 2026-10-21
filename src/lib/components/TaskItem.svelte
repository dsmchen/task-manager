<script lang="ts">
	import type { Task } from '#lib/types.ts';

	let { task, onDelete }: { task: Task; onDelete: () => void } = $props();
</script>

<label>
	<input type="checkbox" bind:checked={task.done} />
	<span class:done={task.done}>{task.text}</span>
</label>
<button type="button" class="del" aria-label={`Delete: ${task.text}`} onclick={onDelete}>
	<svg
		viewBox="0 0 24 24"
		width="16"
		height="16"
		fill="none"
		stroke="currentColor"
		stroke-width="2"
		stroke-linecap="round"
		stroke-linejoin="round"
		aria-hidden="true"
	>
		<path d="M3 6h18M8 6V4h8v2M19 6l-1 14H6L5 6M10 11v5M14 11v5" />
	</svg>
</button>

<style>
	.done {
		text-decoration: line-through;
		color: #666;
	}

	.del {
		flex: none;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		width: 1.75rem;
		height: 1.75rem;
		padding: 0;
		border: none;
		border-radius: 4px;
		background: none;
		color: #767676; /* 4.54:1 — clears the 3:1 non-text bar */
		cursor: pointer;
	}
	.del:hover {
		background: #fdecec;
		color: #a33; /* 6.50:1 */
	}
	.del:focus-visible {
		outline: 2px solid #a33;
		outline-offset: 2px;
	}
</style>
