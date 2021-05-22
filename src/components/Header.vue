<template>
	<header>
		<h1>{{ title }}</h1>
		<Button
			v-show="isHomePage"
			@btn-click="$emit('toggle-add')"
			:text="showAddTask ? 'Close' : 'Add Task'"
			:color="showAddTask ? 'red' : 'green'"
		/>
	</header>
</template>

<script>
import Button from './Button';
export default {
	name: 'Header',
	props: {
		title: String,
		showAddTask: Boolean,
	},
	components: {
		Button,
	},
	computed: {
		isHomePage() {
			console.log('akira ', this.$route.path);
			if (this.$route.path === '/') {
				return true;
			} else {
				return false;
			}
		},
	},
};
</script>

<style scoped>
header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 20px;
}
</style>
