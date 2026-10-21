<script lang="ts">
	interface Task {
		id: number;
		text: string;
		done: boolean;
	}

	let tasks = $state<Task[]>([
		{ id: 1, text: 'First task', done: false },
		{ id: 2, text: 'Second task', done: true }
	]);
</script>

<h1>Task Manager</h1>

{#each tasks as task (task.id)}
	<p>{task.done ? '✓' : '○'} {task.text}</p>
{/each}
