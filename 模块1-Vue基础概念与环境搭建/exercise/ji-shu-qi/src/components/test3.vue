<template>
<div v-if="status==false">
    <h4>{{ loginStatus }}</h4>
    <h4>请输入用户名和密码：</h4>
    <input type="text" placeholder="请输入用户名" v-model="username">
    <input type="password" placeholder="请输入密码" v-model="password">
    <button @click="login">登录</button>
</div>
<div v-else>
    <h4>登录成功，用户名：{{ username }}</h4>
    <h4>登录时间:{{ logintime }}</h4>
    <button @click="logout">退出登录</button>
</div>
</template>


<script setup>
import {ref } from 'vue'
const username = ref('')
const password = ref('')
const loginStatus = ref('未登录')
const status = ref(false)
const logintime = ref()
function login() {
    if (username.value === 'admin' && password.value === '123456') {
      loginStatus.value = '登录成功'
      status.value = true
      logintime.value = new Date().toLocaleString()
    } else {
      loginStatus.value = '登录失败'
    }
  }

  function logout(){
    loginStatus.value = '未登录'
    status.value = false
    logintime.value = ''
  }


</script>