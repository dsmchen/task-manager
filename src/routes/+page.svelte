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
			<label>
				<input type="checkbox" bind:checked={task.done} />
				<span class:done={task.done}>{task.text}</span>
			</label>
			<button
				type="button"
				class="del"
				aria-label={`Delete: ${task.text}`}
				onclick={() => deleteTask(task.id)}
			>
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
		</li>
	{/each}
</ul>
