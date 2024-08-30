<template>
    <h1>To-do list</h1>
    <hr>
    <form action="" @submit.prevent="addTask">
        <fieldset role="group">
            <input v-model="newTask" type="text" placeholder="Tâches à effectuer">
            <button :disabled="newTask.length === 0">Ajouter une tâche</button>
        </fieldset>
    </form>
    <ul v-if="tasks">
        <li v-for="task in sortedTasks()" :key="task.date"
            :style="{ backgroundColor: task.completed ? 'lightgray' : 'lightblue' }">
            <input type="checkbox" v-model="task.completed" :id="task.date" />
            <label :for="task.date" :style="{ textDecoration: task.completed ? 'line-through' : '' }">
                {{ task.title }}
            </label>
        </li>
    </ul>
    <span v-else>Il n'y a pas de tâches</span>
    <div>
        <label>
            <input type="checkbox" v-model="hideCompletedTasks">
            Masquer les tâches complétées
        </label>
    </div>
</template>

<script setup>
import { ref } from 'vue'
const task = ref('')
const newTask = ref('')
const hideCompletedTasks = ref(false)

const sortedTasks = () => {
    const sortedTasks = tasks.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
    if (hideCompletedTasks.value) {
        return sortedTasks.filter(task => task.completed === false);
    }
    return sortedTasks;
}

const addTask = () => {
    tasks.value.push({
        title: newTask.value,
        completed: false,
        date: Date.now()
    });
    newTask.value = '';
}
const tasks = ref([
    { "title": "Acheter la propriété 'Rue de la Paix'", "completed": false, "date": 1725012543826 },
    { "title": "Construire un hôtel sur 'Avenue Foch'", "completed": true, "date": 1325012843826 },
    { "title": "Éviter la case prison", "completed": false, "date": 1725012845826 },
]);
</script>

<style>
* {
    transition: all 0.2s ease-in;
}

h1 {
    color: lightblue;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: flex;
    align-items: center;
    border-radius: 10px;
    height: 50px;
    background-color: bisque;
    margin: 10px;
}

label {
    display: flex;
    align-items: center;
    height: 100%;
    width: 100%;
}
</style>
