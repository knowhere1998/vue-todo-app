<template>
	<div class="container">
		<div class="title">
			<h1>{{ msg }}</h1>
			<label>
				<input type="text" placeholder="New Task goes here..." class="new-task" v-model="newTodo" @keyup.enter="addTodo">
			</label>
		</div>
		<div class="todo-item" v-for="todo in todos" :key="todo.id">
			<div v-if="todo.completed" class="flex-left">
				<span style="text-decoration: line-through;">
					{{ todo.task }}
				</span>
			</div>
			<div v-else>
				{{todo.task }}
			</div>
			<span @click="removeTask(todo.id)" class="flex-right">
				<i class="fa fa-times" aria-hidden="true"></i>
			</span>
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
				nextid: 1,
				todos: []
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
			},
			removeTask(id){
				console.log(id);
				this.todos.pop()
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
