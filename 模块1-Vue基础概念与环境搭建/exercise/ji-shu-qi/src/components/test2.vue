<template>
<div>
    <div>
    <h2>计算器</h2>
    <p>计算结果：{{ result }}</p>
    <p>计算表达式：{{ bds }}</p>
    </div>

    <div>
        <input type="number" v-model.number="num1" placeholder="第一个数字">
        <select v-model="operator">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
        </select>
        <input type="number" v-model.number="num2" placeholder="第二个数字">
        <button @click="calculate">计算</button>
        <button @click="clear">清除</button>
    </div>
    <div v-if="history.length>0">
        <h4>计算历史：</h4>
        <ul>
            <li v-for="item in history">{{ item }}</li>
        </ul>
        <button @click="clearHistory">清除历史</button>
    </div>
</div>
</template>

<script setup>
import {ref} from 'vue'
const result=ref(0);
const bds=ref("")
const num1=ref(0)
const num2=ref(0)
const operator=ref('+')
const history=ref([])

function calculate() {
    let expression = num1.value + ' ' + operator.value + ' ' + num2.value
    bds.value = expression
    switch (operator.value) {
        case '+':
            result.value = num1.value + num2.value
            break
        case '-':
            result.value = num1.value - num2.value
            break
        case '*':
            result.value = num1.value * num2.value
            break
        case '/':
            if (num2.value === 0) {
                result.value = '除数不能为0'
                return
            }
            result.value = num1.value / num2.value
            break
    }
    history.value.unshift(expression + ' = ' + result.value)
    if(history.value.length>5)
    {
        history.value=history.value.slice(0,5)
    }
}

function clear() {
    num1.value = 0
    num2.value = 0
    operator.value = '+'
    result.value = 0
    bds.value = ''
}
function clearHistory() {
    history.value = []
}

</script>