<template>
	<div v-show="showAddTask">
		<AddTask @add-task="addTask" />
	</div>
	<Tasks
		@toggle-reminder="toggleReminder"
		@delete-task="deleteTask"
		:tasks="tasks"
	/>
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
	name: 'Home',
	components: {
		Tasks,
		AddTask,
	},
	props: {
		showAddTask: Boolean,
	},
	data() {
		return {
			tasks: [],
		};
	},
	methods: {
		async deleteTask(id) {
			if (confirm('Are you sure?')) {
				const res = await fetch(`api/tasks/${id}`, {
					method: 'DELETE',
					headers: {
						'Content-type': 'application/json',
					},
					body: JSON.stringify(this.task),
				});

				res.status === 200
					? (this.tasks = this.tasks.filter((task) => {
							return task.id !== id;
					  }))
					: alert('Error deleting task');
			}
		},
		async toggleReminder(id) {
			const taskToToggle = await this.fetchTask(id);
			const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

			const res = await fetch(`api/tasks/${id}`, {
				method: 'PUT',
				headers: {
					'Content-type': 'application/json',
				},
				body: JSON.stringify(updTask),
			});

			const data = await res.json();

			this.tasks = this.tasks.map((task) => {
				if (task.id === id) {
					return { ...task, reminder: data.reminder };
				} else {
					return task;
				}
			});
		},
		async addTask(newTask) {
			const res = await fetch('api/tasks', {
				method: 'POST',
				headers: {
					'Content-type': 'application/json',
				},
				body: JSON.stringify(newTask),
			});

			const data = await res.json();

			this.tasks = this.tasks.concat(data);
		},
		async fetchTasks() {
			// const res = await fetch("http://localhost:5000/tasks");
			const res = await fetch('api/tasks');
			const data = await res.json();

			return data;
		},
		async fetchTask(id) {
			// const res = await fetch(`http://localhost:5000/tasks/${id}`);
			const res = await fetch(`api/tasks/${id}`);
			const data = await res.json();

			return data;
		},
	},
	async created() {
		this.tasks = await this.fetchTasks();
	},
};
</script>

<style></style>
