<template>
	<div id="app" class="container card">
		<div class="card-header-icon">
			<img src="./assets/logo.jpeg" class="logo">
		</div>
		<TodoList/>
	</div>
</template>

<script>
	import TodoList from './components/TodoList'

	export default {
		name: 'App',
		components: {
			TodoList
		}
	}
</script>

<style>

	@import "bulma/css/bulma.css";

	.container {
		max-width: 600px;
		margin: 0 auto;
		padding: 6px 6px 6px 6px;
	}

	html{
		background: linear-gradient(251deg, #3ecdd5, #d90eb7);
		background-size: 200% 200%;

		-webkit-animation: background-animation 19s ease infinite;
		animation: background-animation 19s ease infinite;

		@-webkit-keyframes background-animation {
			0%{background-position:73% 0%}
			50%{background-position:28% 100%}
			100%{background-position:73% 0%}
		}

		@keyframes background-animation {
			0%{background-position:73% 0%}
			50%{background-position:28% 100%}
			100%{background-position:73% 0%}
		}
	}

	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		/*text-align: center;*/
		color: #2c3e50;
		margin-top: 60px;
	}

	.logo {
		display: block;
		margin: 20px auto;
		height: 75px;
	}
</style>
