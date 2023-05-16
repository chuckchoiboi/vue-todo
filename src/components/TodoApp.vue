<template>
	<div class="container">
		<h2 class="text-center mt-5">My Vue Todo App</h2>

		<div class="d-flex">
			<input
				v-model="task"
				type="text"
				placeholder="Enter task"
				class="form-control"
			/>
			<button @click="submitTask" class="btn btn-warning rounded-0">
				{{ buttonText }}
			</button>
		</div>

		<!-- Task table -->
		<table class="table table-bordered mt-5">
			<thead>
				<tr>
					<th scope="col">Task</th>
					<th scope="col">Status</th>
					<th scope="col" class="text-center">#</th>
					<th scope="col" class="text-center">#</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(task, index) in tasks" :key="index">
					<th>
						<span
							:class="{ completed: task.status === 'completed' }"
							>{{ task.name }}</span
						>
					</th>
					<td style="width: 120px">
						<span
							@click="changeStatus(index)"
							class="pointer"
							:class="{
								'text-danger': task.status === 'to-do',
								'text-warning': task.status === 'in-progress',
								'text-success': task.status === 'completed',
							}"
						>
							{{ firstCharUpper(task.status) }}
						</span>
					</td>
					<td>
						<div class="text-center" @click="editTask(index)">
							<span class="fa fa-pen"></span>
						</div>
					</td>
					<td>
						<div class="text-center" @click="deleteTask(index)">
							<span class="fa fa-trash"></span>
						</div>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>
<script lang="ts">
export default {
	name: 'HelloWorld',
	props: {
		msg: String,
	},

	data() {
		return {
			task: '',
			editedTask: null as null | number,
			taskStatus: ['to-do', 'in-progress', 'completed'],
			tasks: [
				{
					name: 'Walk my dog',
					status: 'to-do',
				},
				{
					name: 'Go get groceries',
					status: 'in-progress',
				},
			],
		};
	},

	computed: {
		buttonText() {
			if (this.editedTask) {
				return 'EDIT';
			} else {
				return 'SUBMIT';
			}
		},
	},

	methods: {
		submitTask() {
			if (this.task.length === 0) return;

			if (this.editedTask === null) {
				this.tasks.push({
					name: this.task,
					status: 'to-do',
				});
			} else {
				this.tasks[this.editedTask].name = this.task;
				this.editedTask = null;
			}

			this.task = '';
		},

		deleteTask(index: number) {
			this.tasks.splice(index, 1);
		},

		editTask(index: number) {
			this.task = this.tasks[index].name;
			this.editedTask = index;
		},
		changeStatus(index: number) {
			let newIndex = this.taskStatus.indexOf(this.tasks[index].status);
			if (++newIndex > 2) newIndex = 0;
			this.tasks[index].status = this.taskStatus[newIndex];
		},
		firstCharUpper(str: string) {
			return str.charAt(0).toUpperCase() + str.slice(1);
		},
	},
};
</script>
<style scoped>
.pointer {
	cursor: pointer;
}
.completed {
	text-decoration: line-through;
}
</style>
