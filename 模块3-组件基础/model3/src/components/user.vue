<template>
<div class="user-info">
    <h3>用户信息</h3>
    <p><strong>姓名：</strong>{{ user.name || "未设置"}}</p>
    <p><strong>年龄：</strong>{{ user.age || "未设置"}}</p>
    <p><strong>邮箱：</strong>{{ user.email || "未设置"}}</p>
    <p><strong>部门：</strong>{{ user.department || "未设置"}}</p>
</div>

</template>


<script setup>
import {defineProps} from 'vue'

const props = defineProps({
    user:{
        type:Object,
        required:true,
    }
})

</script>