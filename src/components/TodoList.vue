<template>
	<div class="container card-content">
		<div class="title">
			<h1>{{ msg }}</h1>
				<input type="text" placeholder="New Task goes here..." class="new-task" v-model="newTodo" @keyup.enter="addTodo">
		</div>
		<div class="todo-item" v-for="(todo, index) in filteredTasks" :key="todo.id">
			<div class="todo-item-left">
				<input type="checkbox" v-model="todo.completed">
				<div v-if="!todo.editMode" :class="{ 'completed': todo.completed }" @dblclick="toggleEdit(index)" class="todo-item-label">
					{{ todo.task }}
				</div>
				<input class="todo-item-edit" type="text" v-else v-model="todo.task" @blur="doneEdit(todo)"
					   @keyup.escape="abortEdit(todo)" @keyup.enter="doneEdit(todo)">
			</div>
			<div class="remove-item" @click="removeTask(index)">
				&times;
			</div>
		</div>
		<div class="bottom-container border" v-if="anyTasks !== 0">
			<div>
				<label class="checkbox">
					<input type="checkbox" :checked="!anyRemaining" @change="checkAllTasks">
					Check all
				</label>
			</div>
			<div v-if="remaining !== 0">
				{{ remaining }} items left
			</div>
		</div>
		<div class="bottom-container level" v-if="anyTasks !== 0">
			<div class="buttons level-left">
				<div class="level-item">
					<button class="button is-small is-link" :class="{ 'is-primary': filter === 'all' }" @click="filter = 'all'">All</button>
					<button class="button is-small is-link" :class="{ 'is-primary': filter === 'remaining' }" @click="filter = 'remaining'">Remaining</button>
					<button class="button is-small is-link" :class="{ 'is-primary': filter === 'completed' }" @click="filter = 'completed'">Completed</button>
				</div>
			</div>
			<div class="clear-completed level-right">
				<div class="level-item">
					<button class="button is-small is-danger" v-if="hasCompletedTasks" @click="clearCompleted">Clear Completed</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: "TodoList",
		data () {
			return {
				msg: 'Welcome to TODO Vue.js App',
				newTodo: '',
				nextid: 1,
				beforeEdit: '',
				todos: [],
				filter: 'all'
			}
		},
		mounted: function () {
			console.log("Attmepting to load");
			this.loadData();

			setInterval(function () {
				this.storeData();
			}.bind(this), 3000); 
  		},
		computed:{
			filteredTasks() {
				if (this.filter === 'all') {
					return this.todos
				} else if (this.filter === 'remaining'){
					return this.todos.filter(todo => !todo.completed)
				} else {
					return this.todos.filter(todo => todo.completed)
				}
			},
			remaining(){
				return this.todos.filter(todo => !todo.completed).length
			},
			anyRemaining() {
				return this.remaining !== 0
			},
			anyTasks() {
				return this.todos.length
			},
			hasCompletedTasks() {
				return this.todos.filter(todo => todo.completed).length > 0
			}
		},
		methods:{
			addTodo(){
				if (this.newTodo.trim().length !== 0) {
					this.todos.push({
						id: this.nextid,
						task: this.newTodo.trim(),
						completed: false,
						editMode: false
					});
					this.newTodo = '';
					this.nextid++
				}
			},
			removeTask(id){
				this.todos.splice(id, 1)
			},
			toggleEdit(id){
				this.todos[id].editMode = true;
				this.beforeEdit = this.todos[id].task;
			},
			abortEdit(obj){
				obj.task = this.beforeEdit;
				obj.editMode = false
			},
			doneEdit(obj){
				if(obj.task.trim().length === 0){
					obj.task = this.beforeEdit
				}
				obj.editMode = false
			},
			checkAllTasks() {
				if (this.anyRemaining) {
					this.todos.forEach((todo) => todo.completed = true)
				} else {
					this.todos.forEach((todo) => todo.completed = false)
				}
			},
			clearCompleted(){
				this.todos = this.todos.filter(todo => !todo.completed)
			},
			loadData() {
				if ($cookies.isKey("tasks")){
					if ($cookies.get("tasks").length > 0) {
						this.todos = JSON.parse($cookies.get("tasks"));
						console.log(JSON.parse($cookies.get("tasks")));
					}
				};
			},
			storeData(){
				if (this.todos.length > 0){
					$cookies.set("tasks", JSON.stringify(this.todos));
					console.log(JSON.stringify(this.todos));
				}
			}
		}
	}
</script>

<style>
	.title{
		text-align: center;
		margin-bottom: 15px;
	}

	h1{
		font-weight: bolder;

		padding-bottom: 20px;
	}

	.completed {
		text-decoration: line-through;
		color: gray;
	}

	.todo-item {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 5px 18px;
		width: 100%;
	}

	.todo-item-left {
		margin-bottom: 12px;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.todo-item-label {
		padding: 10px;
		border: 1px solid white;
		margin-left: 12px;
	}

	.todo-item-edit {
		font-size: 24px;
		border: 1px solid #ccc;
		width: 100%;
		color: #2c3e50;
	}

	.remove-item:hover {
		color: red;
	}

	.new-task {
		width: 100%;
		padding: 10px 18px;
		font-size: 18px;
	}

	&:focus {
		outline: 0;
	}

	.bottom-container{
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 16px;
		padding: 10px 18px 0 18px;
		width: 100%;
		margin-bottom: 8px;
	}

	.border{
		border-top: 1px solid lightgray;
	}

	.active {
		background-color: lightblue;
	}

	button{
		background-color: #fff;
		-webkit-appearance: none;
		-moz-appearance: none;
		appearance: none;
		font-size: 16px;
	}

</style>
