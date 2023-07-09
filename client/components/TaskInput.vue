<template>
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

    <v-alert v-if="watchToggle" type="success">
        <v-alert-title>All tasks completed, Nice!</v-alert-title>
    </v-alert>

    <v-row class="my-2 mx-3">
        <strong class="ml-3">Remaining: {{ remaining }}</strong>
        <strong class="ml-6">Completed: {{ completed }}</strong>
        <br>
        <v-spacer />
        <v-progress-circular v-model="progress" :color="progress == 100 ? 'green' : 'orange-darken-2'"></v-progress-circular>
    </v-row>
</template>

<script setup lang="ts">

const id = ref(4)

const props = defineProps({
    watchToggle: Boolean,
    taskList: Object,
    completed: {
        type: Number,
        default: 0
    },
    remaining: {
        type: Number,
        default: 0
    },
    progress: Number
})

const progress = computed(()=> (props.completed / props.taskList?.length ) * 100)

const modelTask = reactive({
	newTask: '',
	rules: [
		(value: boolean) => {
			if (value) return true
			return 'Enter a task'
		},
	],
})

function addNewTask() {
	const addedTask = {
		id: id.value++,
		isDone: false,
		task: modelTask.newTask,
	}

	if (modelTask.newTask == '') {
		return
	} else {
		props.taskList?.push(addedTask)
		modelTask.newTask = ''
	}
}



</script>