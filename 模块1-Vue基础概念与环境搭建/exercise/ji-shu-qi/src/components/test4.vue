<template>
<div>
    <div>
    <label for="Task">添加新任务：</label>
    </div>
    <div>
    <input type="text" id="Task" v-model="newTask" placeholder="请输入新任务">
    <button @click="addTask">添加</button>
    </div>

    <div>
        <h4>代办事项(共{{todos.length }}项)</h4>
        <div v-if="todos.length===0">
            <p>暂无任务</p>
        </div>

        <ul v-else>
            <li v-for="(todo,index) in todos" :key="todo.id">
             <input type="checkbox" v-model="todo.completed">
             {{ todo.text }}
             <span v-if="todo.completed">已完成</span>
             <button @click="removeTask(index)">删除</button>
            
            </li>
        </ul>
        <div v-if="todos.length>0">
            <p><strong>完成进度：</strong>{{completedCount}}/{{todos.length}}({{ completionPercentage }}%)</p>
            <button @click="clearCompleted">清除已完成任务</button>
            <button @click="clearAll">清除所有任务</button>
        </div>
    </div>
</div>
</template>



<script setup>
import {ref,computed} from 'vue'
const newTask=ref('')
const todos=ref([
    {id :1,text:"学习vue",completed:false},
    { id: 2, text: '完成练习题', completed: false },
    { id: 3, text: '阅读官方文档', completed: true }
])
const completedCount=computed(()=>{
    return todos.value.filter(todo=>todo.completed).length
})
const completionPercentage=computed(()=>{
    return todos.value.length===0?0:Math.round(completedCount.value/todos.value.length*100)
})
function addTask() {
    if (newTask.value.trim() !== '') {
      todos.value.push({
        id: todos.value.length + 1,
        text: newTask.value,
        completed: false
      })
      newTask.value = ''
    }
  }
function removeTask(index) {
    todos.value.splice(index, 1)
  }
function clearCompleted() {
    todos.value = todos.value.filter(todo => !todo.completed)
  }
function clearAll() {
    todos.value=[]
}

</script>