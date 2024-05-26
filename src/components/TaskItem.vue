<template>
    <div @click="toggleState" class="task" :class="stateClass">
        <span @click.stop="$emit('taskDeleted', task)" class="close">X</span>
        <p v-if="!editing" @click="startEditing">{{ task.name }}</p>
        <div v-else>
            <input type="text" v-model="newName" @keydown.enter="finishEditing" @blur="finishEditing">
            <button @click="finishEditing">OK</button>
        </div>
    </div>
</template>

<script>
/* eslint-disable */
export default {
    props: {
        task: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            editing: false,
            newName: ''
        };
    },
    computed: {
        stateClass() {
            return {
                pending: this.task.pending,
                done: !this.task.pending
            };
        }
    },
    methods: {
        startEditing() {
            this.editing = true;
            this.newName = this.task.name;
        },
        finishEditing() {
            this.editing = false;
            if (this.newName.trim() !== '') {
                this.task.name = this.newName.trim();
                this.$emit('nameChanged', this.task);
            }
        },
        toggleState(event) {
            // Verificar se o usuário está editando o nome da tarefa
            if (!this.editing) {
                this.$emit('taskStateChanged', this.task);
            }
        }
    }
};
</script>

<style scoped>
    .task{
        position: relative;
        box-sizing: border-box;
        width: 350px;
        height: 150px;
        padding: 10px;
        border-radius: 8px;
        font-size: 2rem;
        font-weight: 300;
        cursor: pointer;
        user-select: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pending{
        border-left: 12px solid #B73228;
        background-color: #f44336;
    }

    .done{
        color: #DDD;
        border-left: 12px solid #0a8F08;
        background-color: #4CAF50;
        text-decoration: line-through;
    }
    .pending .close {
        background-color: #B73229;
    }
    .done .close {
        background-color: #0A8F08;
    }
    .close {
        position: absolute;
        right: 10px;
        top: 10px;
        font-size: 0.9rem;
        font-weight: 600;
        height: 20px;
        width: 20px;
        border-radius: 10px;
        display: flex;
        justify-content: center;
    }
</style>