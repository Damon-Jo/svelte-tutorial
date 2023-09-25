<script>
	let todos = [];
	let task = "";


	const addTodo = () => {
		let todo = {
		task: task,
		isComplete: false,
		createdAtm: new Date(),
	}

		todos = [...todos, todo];
		task="";
	}

	// $: console.log(todo);
	$: console.table(todos);

</script>

<main>

	<input type="text" placeholder="Add a task" bind:value={task}/>
	<button on:click={addTodo}>Add</button>
	<ol>
		{#each todos as todo}
			<li class:complete={todo.isComplete}>{todo.task}</li>
		{/each}
	</ol>
</main>

<style>
	.complete {
		text-decoration: line-through;
	}

	/* main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	} */
</style>