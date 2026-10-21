<script lang="ts">
	interface Task {
		id: number;
		text: string;
		done: boolean;
	}

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
</script>

<h1>Task Manager</h1>

<form
	onsubmit={(e) => {
		e.preventDefault();
		addTask();
	}}
>
	<input type="text" bind:value={newTaskText} placeholder="New task..." />
	<button type="submit">Add</button>
</form>

{#each tasks as task (task.id)}
	<p>{task.done ? '✓' : '○'} {task.text}</p>
{/each}
