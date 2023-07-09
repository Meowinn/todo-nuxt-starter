<template>
    <v-card-text>
        <v-list-item class="ma-0 pa-0" link :ripple="false" v-for="(allTask, index) in taskSet" :key="index">
            <v-checkbox v-model="allTask.isDone" color="red">
                <template #label>
                    <p :class="allTask.isDone ? 'strike' : ''" class="align-self-center">{{ allTask.task }}</p>
                </template>
                <template #append>

                    <v-dialog width="344" persistent>
                        <template #activator="{ props }">
                            <v-btn v-bind="props" variant="text" color="yellow-darken-3" icon="mdi-pencil" size="small"
                                @click="edited = allTask.task"></v-btn>
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
                                    <v-btn @click="allTask.task = edited; isActive.value = !isActive">Save</v-btn>
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
</template>

<script setup lang="ts">
const props = defineProps({
    taskSet: Object
})

const taskSet = props.taskSet

const edited = ref('');

function deleteTask(allTaskId: any) {
	taskSet?.splice(taskSet.findIndex((task:any) => task.id === allTaskId), 1)
}

</script>

<style scoped>
.strike {
	text-decoration: line-through;
}
</style>