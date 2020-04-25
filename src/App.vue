<template>
	<div id="app">
		<Table />
	</div>
</template>

<script>
	import Table from './components/Table.vue'

	export default {
		name: 'App',
		components: {
			Table
		}
	}
</script>

<style>
	#app {
		margin: 0;
		padding: 0;
		border: none;
	}
</style>
