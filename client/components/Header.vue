<template>
    <div class="bg-deep-orange-darken-2 py-3">
        <v-card-title>{{ days[day] }}, {{ date }}</v-card-title>
        <v-card-subtitle>{{ months[currentDate.getMonth()] }}</v-card-subtitle>
    </div>
</template>

<script setup lang="ts">
const currentDate = new Date();
const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
let day = currentDate.getDay();
const date = `${currentDate.getDate()}`


</script>