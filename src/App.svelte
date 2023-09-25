<script>
	let todos = [];
	let task = "";
	let error = "";


	const addTodo = () => {
		let todo = {
		task: task,
		isComplete: false,
		createdAtm: new Date(),
	};
	if(task !== ""){
		todos = [...todos, todo];
		error = "";
	} else {
		error = "Task is empty"
	}
		task="";
	}

	const markTodoAsComplete = (index) => {
		todos[index].isComplete = !todos[index].isComplete
	}

	const deleteTodo = (index) => {
		let deleteItem = todos[index]

		todos = todos.filter((item)=> item != deleteItem);
	};

	const keyIsPressed = (event) => {
		if(event.key === "Enter"){
			addTodo();
		}
	}

	// $: console.log(todo);
	$: console.table(todos);

</script>

<main>
	<input type="text" placeholder="Add a task" bind:value={task}/>
	<button on:click={addTodo}>Add</button>
	<ol>
		{#each todos as todo, index}
			<li class:complete={todo.isComplete}>
				<span>
					{todo.task}
				</span>
				<span>
					<button on:click={() => markTodoAsComplete(index)}>✓</button>
					<button on:click={() => deleteTodo(index)}>✘</button>
				</span>
			</li>
		{:else}
			<p>No todos found</p>
		{/each}
		<p class="error">{error}</p>
	</ol>
</main>
<svelte:window on:keydown={keyIsPressed} />

<style>
	.complete {
		text-decoration: line-through;
	}

	.error {
		color: red;
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