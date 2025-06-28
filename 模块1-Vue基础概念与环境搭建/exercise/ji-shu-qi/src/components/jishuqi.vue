<template>
  <div>
    <div>
    <h2>当前计数：{{ count }}</h2>
    </div>

    <div>
        <label for="baseCount">
            步长：
        </label>
    <input id="baseCount" type="number" v-model.number="baseCount">
    </div>

    <div>
    <button @click="increase">增加</button>
    <button @click="decrease">减少</button>
    <button @click="reset">重置</button>
    </div>


    <div>
        <h1>历史记录</h1>
        <ul>
            <li v-for="item in history">{{ item }}</li>
        </ul>
    </div>
    
  </div>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)
const baseCount = ref(1)
const history = ref([])

function logHistory() {
  let time = new Date().toLocaleString()
  history.value.unshift(time + '：值为 ' + count.value) // 使用 unshift 把最新记录放在最前面

  // 限制最多保存5条记录
  if (history.value.length > 5) {
    history.value = history.value.slice(0, 5)
  }
}
function increase() {
  count.value += baseCount.value
  logHistory()
}

function decrease() {
  count.value -= baseCount.value
  logHistory()
}

function reset() {
  count.value = 0
  logHistory()
}
</script>


<style scoped>

</style>