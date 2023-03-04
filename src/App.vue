<template>
	<Form title="Список задач"></Form>
</template>

<script>
	import Form from './components/Form.vue'
	
	export default{
		components: {
			Form
		},
	};
</script>

<style lang="css">
	@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300;400;500;700&display=swap');
	*{
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		user-select: none;
		outline: none;
		font-family: 'Comfortaa';
	}

	body{background: #fcfcfc;}
</style>