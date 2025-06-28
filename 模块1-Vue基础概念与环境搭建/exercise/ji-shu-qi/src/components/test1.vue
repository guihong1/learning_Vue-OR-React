<template>
<div>
    <div>
        <h4>个人信息卡片</h4>
        <p><strong>姓名：</strong>{{ name }}</p>
        <p><strong>年龄：</strong>{{ age }}</p>
        <p><strong>性别：</strong>{{ sex }}</p>
        <p><strong>职业：</strong>{{ job }}</p>
        <p><strong>自我介绍：</strong>{{introduction }}</p>

    </div>

    <div>
        <h4>修改信息</h4>
        <input v-model="name" placeholder="请输入姓名">
        <br>
        <input v-model.number="age" type="number" placeholder="请输入年龄">
        <br>
        性别：
        <select v-model="sex">
            <option value="男">♂</option>
            <option value="女">♀</option>
        </select>
        <br>
        <input v-model="job" placeholder="请输入职业">
        <br>
        <textarea v-model="introduction" placeholder="请输入自我介绍"></textarea>
    </div>

</div>

</template>

<script setup>
import {ref} from 'vue'
const name = ref('张三')
const age = ref(18)
const sex = ref('男')
const job = ref('学生')
const introduction = ref('开心活泼')

</script>


