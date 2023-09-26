<script>

	import { initializeApp, getApps, getApp} from "firebase/app";
	import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc, addDoc} from "firebase/firestore";
	import {firebaseConfig} from './lib/firebaseConfig';


	let firebaseApp;
	let db;

	// 	if(getApp().length === 0){
	// 		firebaseApp = initializeApp(firebaseConfig);
	// 	} else {
	// 		getApp();
	// 	}
	// 	db = getFirestore();

	

	// // Initialize Cloud Firestore and get a reference to the service
	
	// console.log({firebaseApp, db})

	if (!firebaseApp) {
    firebaseApp = initializeApp(firebaseConfig);
  }
  // Initialize Firestore
  db = getFirestore();

  const colRef = collection(db, "todos")

  let todos = [];

  const unsubscribe = onSnapshot(colRef, (querySnapshot) => {
	let fbTodos = []
  querySnapshot.forEach((doc) => {
	let todo = {...doc.data(), id: doc.id}
    fbTodos = [todo, ...fbTodos];
  });
  todos = fbTodos;
});


	let task = "";
	let error = "";


	const addTodo = async () => {
	// 	let todo = {
	// 	task: task,
	// 	isComplete: false,
	// 	createdAt: new Date(),
	// };
	if(task !== ""){
		// todos = [...todos, todo];

		// Add a new document with a generated id.
		const docRef = await addDoc(collection(db, "todos"), {
			task: task,
			isComplete: false,
			createdAt: new Date(),
		});
		
		error = "";
	} else {
		error = "Task is empty"
	}
		task="";
	}

	const markTodoAsComplete = async (item) => {
		// todos[index].isComplete = !todos[index].isComplete
		// Set the "capital" field of the city 'DC'
		await updateDoc(doc(db, "todos", item.id), {
			isComplete: !item.isComplete
});
	}

	const deleteTodo = async (id) => {
		// let deleteItem = todos[index]

		// todos = todos.filter((item)=> item != deleteItem);
		await deleteDoc(doc(db, "todos", id));
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
		{#each todos as todo}
			<li class:complete={todo.isComplete}>
				<span>
					{todo.task}
				</span>
				<span>
					<button on:click={() => markTodoAsComplete(todo)}>✓</button>
					<button on:click={() => deleteTodo(todo.id)}>✘</button>
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