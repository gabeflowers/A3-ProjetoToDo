<template>
    <div class="task-grid">
        <template v-if="tasks.length">
            <Task v-for="(task, i) in tasks" :key="task.name"
                @taskDeleted="$emit('taskDeleted', i)"
                @taskStateChanged="$emit('taskStateChanged', i)"
                @renameRequested="$emit('taskDeleted', i)"
                :task="task"></Task>
        </template>
        <p v-else class="no-task"> Sua vida está em dia :)) </p>
    </div>
</template>

<script>
import Task from './TaskItem.vue'

export default {
    components: { Task },
    
    props: {
        tasks: {
            type: Array,
            required: true,
        }
    },
    methods: {
        renameTask(taskIndex, newName) {
            this.$emit('renameTask', { taskIndex, newName });
        }
    }
}
</script>

<style scoped>
    .task-grid {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }

    .task-grid .task {
        margin: 10px;
    }

    .no-task {
        color: aliceblue;
        font-size: 3rem;
    }
</style>