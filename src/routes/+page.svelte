<script lang="ts">
	import TaskItem from '#lib/components/TaskItem.svelte';
	import type { Task } from '#lib/types.ts';
	import { fade } from 'svelte/transition';
	import { flip } from 'svelte/animate';
	import { prefersReducedMotion } from 'svelte/motion';

	let lastId = 0;
	const newId = () => ++lastId;

	let tasks = $state<Task[]>([
		{ id: newId(), text: 'First task', done: false },
		{ id: newId(), text: 'Second task', done: true }
	]);

	let newTaskText = $state('');

	function addTask() {
		const title = newTaskText.trim();
		if (!title) return;
		tasks.push({ id: newId(), text: title, done: false });
		newTaskText = '';
	}

	function deleteTask(id: number) {
		tasks = tasks.filter((t) => t.id !== id);
	}

	type Filter = 'all' | 'active' | 'done';

	const FILTERS: Filter[] = ['all', 'active', 'done'];

	const filterFns: Record<Filter, (t: Task) => boolean> = {
		all: () => true,
		active: (t) => !t.done,
		done: (t) => t.done
	};

	const emptyMessages: Record<Filter, string> = {
		all: 'No tasks. Add one above.',
		active: "No active tasks. Everything's done.",
		done: 'No completed tasks yet.'
	};

	let filter = $state<Filter>('all');
	let visibleTasks = $derived(tasks.filter(filterFns[filter]));
	let emptyMessage = $derived(tasks.length === 0 ? emptyMessages.all : emptyMessages[filter]);
</script>

<h1>Task Manager</h1>

<form
	onsubmit={(e) => {
		e.preventDefault();
		addTask();
	}}
>
	<label for="new-task" class="sr-only">New task</label>
	<input id="new-task" type="text" bind:value={newTaskText} />
	<button type="submit">Add task</button>
</form>

<div class="filters" role="group" aria-label="Filter tasks">
	{#each FILTERS as f (f)}
		<button
			type="button"
			class:active={filter === f}
			aria-pressed={filter === f}
			onclick={() => {
				filter = f;
			}}>{f}</button
		>
	{/each}
</div>

<ul class="task-list" role="list">
	{#each visibleTasks as task (task.id)}
		<li
			transition:fade={prefersReducedMotion.current ? { duration: 0 } : { duration: 150 }}
			animate:flip={prefersReducedMotion.current ? { duration: 0 } : {}}
		>
			<TaskItem {task} onDelete={() => deleteTask(task.id)} />
		</li>
	{:else}
		<li class="empty">
			{emptyMessage}
		</li>
	{/each}
</ul>
