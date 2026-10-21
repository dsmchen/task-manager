<script lang="ts">
	import TaskItem from '#lib/components/TaskItem.svelte';
	import type { Task } from '#lib/types.ts';

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

<ul class="task-list" role="list">
	{#each tasks as task (task.id)}
		<li>
			<TaskItem {task} onDelete={() => deleteTask(task.id)} />
		</li>
	{/each}
</ul>
