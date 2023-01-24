<script>
	import { object_without_properties } from 'svelte/internal';
	import { fly } from 'svelte/transition';

	let newItem = '';
	let todoItemsContainer = [];
	let isChecked = false;
	let index;
	// Add todo function
	function addTodo() {
		if (todoItemsContainer.length === 0) {
			index = 0;
		} else {
			index += 1;
		}
		const todoItem = {
			text: newItem,
			checked: isChecked,
			id: index
		};
		if (todoItem.text !== '') {
			todoItemsContainer = [...todoItemsContainer, todoItem];
		} else {
			alert('Input some data');
		}
		console.log(todoItemsContainer);
		newItem = '';
	}
	// Remove todo function
	function removeTodo(id) {
		todoItemsContainer = todoItemsContainer.filter((input) => input.id !== Number(id));
		console.log(todoItemsContainer);
	}
</script>

<div class="container">
	<div class="headers">
		<h1 id="todo-header">TODO LIST</h1>
		<h1>📋</h1>
	</div>
	<!-- Take input for the todo list -->
	<form id="add-todo-form" on:submit|preventDefault={addTodo}>
		<input
			type="text"
			bind:value={newItem}
			name="todoItem"
			id="todo"
			placeholder="Input your todo activity"
		/>
		<button type="button" id="submitBtn" on:click|preventDefault={addTodo} > Add new item </button>
	</form>
	<!-- Iterate over the items of the todo list -->
	<div id="todoDisplay">
		{#each todoItemsContainer as todo (todo.id)}
			<div class="iterator" transition:fly={{ y: 200, duration: 500 }}>
				<div class="inner-container">
					<input type="checkbox" name="checked" id="checkbox" bind:checked={todo.checked} />
					<li class="text-displayed" class:checked={todo.checked}>
						{todo.text}
					</li>
				</div>
				<button class="delete-todo" on:click={() => removeTodo(todo.id)}>🗙</button>
			</div>
		{/each}
	</div>
</div>

<style>
	#add-todo-form {
		margin: 3rem 0;
	}
	.container {
		margin-top: 3rem;
		min-height: 100vh;
	}
	form {
		display: flex;
		flex-direction: column;
	}
	#todoDisplay {
		display: flex;
		flex-direction: column;
	}
	li {
		list-style: none;
	}
	.iterator {
		display: flex;
		justify-content: space-between;
		align-items: center;
		border: 1px solid rgb(202, 200, 200);
		border-radius: 10px;
		padding: 1rem;
		margin: 0.5rem 0;
	}
	.text-displayed {
		padding-left: 1rem;
	}
	.inner-container {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.delete-todo {
		color: red;
		outline: none;
		border: none;
		font-size: 24px;
	}
	.checked {
		text-decoration: line-through;
	}
	.headers {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	#todo-header {
		color: rgb(109, 107, 107);
	}
</style>
