<template>
	<v-app>
		<v-card width="500" class="mx-auto my-3">
			<Header />
			<TaskInput 
			:taskList="allTasks" 
			:watchToggle="alertToggle" 
			:completed="completedTasks" 
			:remaining="remainingTasks"
			/>
			
			<v-divider></v-divider>

			<Tasks :taskSet="allTasks" />	
		</v-card>
	</v-app>
</template>
<script lang="ts" setup>

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

</script>


