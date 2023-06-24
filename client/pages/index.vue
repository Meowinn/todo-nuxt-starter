<template>
	<v-app>
		<h2>Todo Starter</h2>

		<v-card width="500" class="mx-auto">
			<v-card-title class="pb-0">{{ days[day] }}, {{ date }}</v-card-title>
			<v-card-subtitle>{{ months[currentDate.getMonth()] }}</v-card-subtitle>
			<v-container fluid>
				<v-form @submit.prevent>
					<v-text-field v-model="modelTask.newTask" :rules="modelTask.rules" density="compact" clearable
						label="Your tasks" type="text" variant="outlined">
						<template #append>
							<v-btn icon="mdi-plus" @click="addNewTask" type="submit"></v-btn>
						</template>
					</v-text-field>
				</v-form>

			</v-container>

			<v-card-text>
				<v-list-item class="ma-0 pa-0" link :ripple="false" v-for="(allTask, index) in allTasks" :key="index">
					<v-checkbox v-model="allTask.isDone" color="red">
						<template #label>
							<p :class="allTask.isDone ? 'strike' : ''">{{ allTask.task }}, {{ allTask.id }}</p>
						</template>
					</v-checkbox>
				</v-list-item>
				<!-- <v-checkbox :label="checkbox.toString()" v-model="checkbox"></v-checkbox> -->
			</v-card-text>

		</v-card>

	</v-app>
</template>
<script lang="ts" setup>

const currentDate = new Date();
const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
let day = currentDate.getDay();
const date = `${currentDate.getDate()}th`

const id = ref(4)

const modelTask = reactive({
	newTask: '',
	rules: [
		value => {
			if (value) return true
			return 'Enter a task'
		},
	],
})

const allTasks = reactive([
	{
		id: 1,
		isDone: false,
		task: 'Doing laundry'
	},
	{
		id: 2,
		isDone: false,
		task: 'Meet with the Kardashians'
	},
	{
		id: 3,
		isDone: false,
		task: 'Join chef ratata'
	},
])

const checkbox = ref(false);

function addNewTask() {
	const addedTask = {
		id: id.value++,
		isDone: false,
		task: modelTask.newTask,
	}


	if (modelTask.newTask == '') {
		return
	} else {
		allTasks.push(addedTask)
		modelTask.newTask = ''
	}

	console.log(addedTask)
}


</script>

<style scoped>
.strike {
	text-decoration: line-through;
}
</style>
