<template>
	<v-app>
		<v-card width="500" class="mx-auto my-3">
			<Header />

			<v-container fluid class="bg-grey-lighten-3">
				<v-form @submit.prevent>
					<v-text-field v-model="modelTask.newTask" :rules="modelTask.rules" density="compact" clearable
						label="Your tasks" type="text" variant="outlined" color="red align-self-center">
						<template #append>
							<v-btn icon="mdi-plus" @click="addNewTask" type="submit" color="orange-darken-1"></v-btn>
						</template>
					</v-text-field>
				</v-form>
			</v-container>

			<v-alert v-if="alertToggle" type="success">
				<v-alert-title>All tasks completed, Nice!</v-alert-title>
			</v-alert>

			
			<v-row class="my-2 mx-3">
				<strong class="ml-3">Remaining: {{ remainingTasks }}</strong>
				<strong class="ml-6">Completed: {{ completedTasks }}</strong>
				<v-spacer />
				<v-progress-circular v-model="progress" :color="progress==100 ? 'green' : 'orange-darken-2	'"></v-progress-circular>
			</v-row>

			<v-divider></v-divider>
			<v-card-text>
				<v-list-item class="ma-0 pa-0" link :ripple="false" v-for="(allTask, index) in allTasks" :key="index">
					<v-checkbox v-model="allTask.isDone" color="red">
						<template #label>
							<p :class="allTask.isDone ? 'strike' : ''" class="align-self-center">{{ allTask.task }}</p>
						</template>
						<template #append>

							<v-dialog width="344" persistent>
								<template #activator="{ props }">
									<v-btn v-bind="props" variant="text" color="yellow-darken-3" icon="mdi-pencil"
										size="small" @click="edited=allTask.task"></v-btn>
								</template>
								<template #default="{ isActive }">
									<v-card>
										<v-toolbar color="warning" title="Edit"></v-toolbar>
										<v-card-text class="pb-0">
											<v-text-field v-model="edited" clearable variant="outlined"
												label="Your task here"></v-text-field>
										</v-card-text>
										<v-card-actions class="justify-end">
											<v-btn @click="isActive.value = !isActive">Close</v-btn>
											<v-btn @click="allTask.task=edited; isActive.value = !isActive">Save</v-btn>
										</v-card-actions>
									</v-card>
								</template>
							</v-dialog>


							<v-btn variant="text" icon="mdi-close" color="error" size="small"
								@click="deleteTask(allTask.id)"></v-btn>
						</template>
					</v-checkbox>
				</v-list-item>
			</v-card-text>
		</v-card>
	</v-app>
</template>
<script lang="ts" setup>


const id = ref(4)

const modelTask = reactive({
	newTask: '',
	rules: [
		(value: boolean) => {
			if (value) return true
			return 'Enter a task'
		},
	],
})

const allTasks = reactive([
	{
		id: 1,
		isDone: false,
		task: 'Do laundry'
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
}

function deleteTask(allTaskId: any) {
	allTasks.splice(allTasks.findIndex(task => task.id === allTaskId), 1)
}

const edited = ref('');

const alertToggle = ref(false)

watch(
	() => (allTasks.every(task => task.isDone)),
	(done) => {
		if (done == true) {
			alertToggle.value = true
		} else {
			alertToggle.value = false
		}
	}
)

const completedTasks = computed(() => allTasks.filter(task => task.isDone).length)
const remainingTasks = computed(()=> allTasks.length - completedTasks.value)
const progress = computed(()=> (completedTasks.value / allTasks.length) * 100) 
</script>

<style scoped>
.strike {
	text-decoration: line-through;
}
</style>
