<script setup>
import GreetingCard from './components/GreetingCard.vue'
import mj from './components/mj.vue'
import UserInfo from './components/user.vue'
import {ref} from 'vue'
const user=ref({
    name:'',
    age:0,
    email:'',
    department:''
})
</script>

<template>
  <div>
 <div class="controls" >
  <label >姓名</label>
  <input v-model="user.name" placeholder="请输入姓名">
  <label >年龄</label>
  <input v-model="user.age" placeholder="请输入年龄">
  <label >邮箱</label>
  <input v-model="user.email" placeholder="请输入邮箱">
  <label >部门</label>
  <input v-model="user.department" placeholder="请输入部门">
 </div>

 <div class="demo-area">
  <UserInfo :user="user" />
 </div>


 </div>
 
</template>

<style scoped>
 body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        .header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 2px solid #f0f0f0;
        }
        
        .header h1 {
            color: #333;
            margin: 0;
            font-size: 2.5em;
        }
        
        .header p {
            color: #666;
            margin: 10px 0 0 0;
            font-size: 1.1em;
        }
        
        .exercise {
            margin-bottom: 40px;
            padding: 25px;
            border: 2px solid #e3f2fd;
            border-radius: 10px;
            background: #fafafa;
        }
        
        .exercise h3 {
            color: #1976d2;
            margin-top: 0;
            font-size: 1.4em;
        }
        
        .requirements {
            background: #fff3e0;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            border-left: 4px solid #ff9800;
        }
        
        .requirements h4 {
            margin: 0 0 10px 0;
            color: #e65100;
        }
        
        .requirements ul {
            margin: 0;
            padding-left: 20px;
        }
        
        .demo-area {
            border: 2px dashed #ddd;
            padding: 20px;
            margin: 20px 0;
            border-radius: 8px;
            background: white;
            min-height: 100px;
        }
        
        .greeting-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            margin: 10px 0;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        
        .greeting-card h2 {
            margin: 0 0 10px 0;
            font-size: 1.8em;
        }
        
        .greeting-card p {
            margin: 0;
            font-size: 1.1em;
            opacity: 0.9;
        }
        
        .user-info {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin: 10px 0;
            border: 1px solid #dee2e6;
        }
        
        .user-info h3 {
            margin: 0 0 10px 0;
            color: #495057;
        }
        
        .user-info p {
            margin: 5px 0;
            color: #6c757d;
        }
        
        .controls {
            margin: 20px 0;
            padding: 15px;
            background: #e8f5e8;
            border-radius: 8px;
        }
        
        .controls label {
            display: block;
            margin: 10px 0 5px 0;
            font-weight: bold;
            color: #2e7d32;
        }
        
        .controls input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 14px;
        }
        
        .tips {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 8px;
            margin: 20px 0;
            border-left: 4px solid #2196f3;
        }
        
        .tips h4 {
            margin: 0 0 10px 0;
            color: #1565c0;
        }
        
        .tips ul {
            margin: 0;
            padding-left: 20px;
        }
        
        .tips li {
            margin: 5px 0;
            color: #1976d2;
        }
</style>
