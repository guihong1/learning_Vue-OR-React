<template>
<div>

    <li class="todo-item" :class="{ completed: todo.completed }">
        <span @click="toggleComplete" style="cursor: pointer; flex: 1;">
            {{ todo.completed ? '✅' : '⭕' }} {{ todo.text }}
        </span>
        <div class="todo-actions">
            <button @click="toggleComplete" class="btn btn-sm" 
                        :class="todo.completed ? 'btn-warning' : 'btn-primary'">
                {{ todo.completed ? '撤销' : '完成' }}
            </button>
            <button @click="deleteItem" class="btn btn-sm btn-danger">
                删除
            </button>
        </div>
    </li>
</div>


</template>


<script setup>
import {defineEmits,defineProps} from 'vue'

const props = defineProps({
    todo: {
        type: Object,
        required: true
    }
})

const emit = defineEmits(['toggle','delete'])

const toggleComplete = () => {
    emit('toggle',props.todo.id)
}
const deleteItem = () => {
    emit('delete',props.todo.id)
}
</script>