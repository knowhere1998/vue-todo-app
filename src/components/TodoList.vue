<template>
	<div class="container">
		<div class="title">
			<h1>{{ msg }}</h1>
			<label>
				<input type="text" placeholder="New Task goes here..." class="new-task" v-model="newTodo" @keyup.enter="addTodo">
			</label>
		</div>
		<div class="todo-item" v-for="todo in todos" :key="todo.id">
			<div class="" v-if="todo.completed"></div>
			{{ todo.task }}
		</div>
	</div>
</template>

<script>
	export default {
		name: "TodoList",
		data () {
			return {
				msg: 'Welcome to Your Vue.js App',
				newTodo: '',
				nextid: 4,
				todos: [
					{
						id: 1,
						task: 'Start Vue App',
						completed: true
					},
					{
						id: 2,
						task: 'Create a TODO List Component',
						completed: false
					},
					{
						id: 3,
						task: 'To make first commit',
						completed: false
					}
				]
			}
		},
		methods:{
			addTodo(){
				this.todos.push({
					id: this.nextid,
					task: this.newTodo,
					completed: false
				});
				this.newTodo = '';
				this.nextid++
			}
		}
	}
</script>

<style>
	.title{
		text-align: center;
	}

	.new-task {
		width: 100%;
		padding: 10px 18px;
		font-size: 18px;
		margin-bottom: 16px;
	}

	&:focus {
		outline: 0;
	}
</style>
