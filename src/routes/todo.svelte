<script>

	let todos = [];
	let todo_title = "";
	let alert_type = "";
	let alert_message = "";
	
	function createTodo() {
		if (todo_title.length < 1) {			
			alert_type = "danger";
			alert_message = "Please input todo!";
			return resetAlert();
		}

		let todo  = {
			title: todo_title,
			id: Date.now(),
			completed: false
		}

		todos = [...todos, todo];
		
		alert_type = "success";
		alert_message = "Successfully add todo";

		todo_title = "";
		return resetAlert();
	}

	function deleteTodo(id) {
		let new_todos = [];
		
		todos.forEach(todo => {
			if (todo.id !== id) {
				new_todos.push(todo)
			}
		})

		todos = new_todos;

		alert_type = "success";
		alert_message = "Successfully remove todo";
		resetAlert();
	}

	function completeTodo(id) {
		todos.forEach(todo => {
			if (todo.id === id) {
				todo.completed = true;
			}
		})

		todos = todos;

		alert_type = "success";
		alert_message = "Successfully complete todo";
		resetAlert();
	}

	function resetAlert() {
		setTimeout(() => {
			alert_type = "";
			alert_message = "";
		}, 1200)
	}
</script>
<svelte:head>
	<title>Todo site</title>
</svelte:head>

<div class="row">
	<div class="col-12">
		<h1>Todo site</h1>
	</div>
	<div class="col-12 mt-5">
		<form on:submit|preventDefault={createTodo}>
			<div class="form-group">
				<input bind:value={todo_title} class="form-control" id="title-input" type="text" name="title" placeholder="Input the title">
			</div>
			<div class="d-grid form-group mt-3">
				<button type="submit" class="btn btn-primary block">Submit</button>
			</div>
		</form>
	</div>
	{#if alert_type !== ""}
		<div class="col-12 mt-4" >
			<div class="alert alert-{alert_type}">
				{alert_message}
			</div>
		</div>
	{/if}
</div>

<div class="row mt-5">
	<div class="col-12">
		<h3>List of Todo</h3>
	</div>
	<div class="col-12 mt-1">

		{#each todos as todo (todo.id)}
			<div class="card mt-2">
				<div class="card-body {todo.completed ? 'bg-success' : ""}">
					<div class="row">
						<div class="col">
							<p class="card-text {todo.completed ? 'text-white' : ""}">{todo.title}
						</div>
						<div class="col text-end">
							<div class="btn-group" role="group" aria-label="Basic example">
								{#if !todo.completed}
									<button type="button" class="btn btn-success" on:click="{completeTodo(todo.id)}"><i class='bx bx-check'></i></button>
								{/if}
								<button type="button" class="btn btn-danger" on:click="{deleteTodo(todo.id)}"><i class='bx bxs-trash-alt'></i></button>
							</div>
							
						</div>
					</div>
				</div>
			</div>		
		{:else}
			<div class="card">
				<div class="card-body">
					<p class="card-text">You have no todo list, go create one!</p>
				</div>
			</div>		
		{/each}
		
	</div>
</div>