<template>
<div class="demo-area" id="app">
    <div class="counter-container">
        <h3>
            计数器
        </h3>
        <div class="counter-display">{{ totalCount }}</div>
        <div class="counter-controls">
            <counter-button
            label="+1"
            :step="1"
            @increment="handleIncrement">
            </counter-button>
             <counter-button
            label="+5"
            :step="5"
            @increment="handleIncrement">
            </counter-button>
             <counter-button
            label="+10"
            :step="10"
            @increment="handleIncrement">
            </counter-button>

             <button @click="resetCounter" class="btn btn-warning">重置</button>
        </div>
       
    </div>

    <div class="message-board">
        <h4>操作记录</h4>
        <div v-if="messages.length === 0" style="text-align:center;color: #666;">
           暂无操作记录
        </div>
        <div v-for="message in messages" :key="message.id" class="message-item">
           <span class="timestamp">{{ message.time }}</span>
           {{ message.text }}
        </div>
    </div>



</div>

</template>


<script setup>
import { ref } from 'vue'
import counterButton from '../components/Counter-button.vue'
const totalCount=ref(0)
const messages=ref([])
const messageId=ref(1)
const handleIncrement=(step)=>{
    totalCount.value+=step
    addMessage(`计数器增加了 ${step}，当前总数：${totalCount.value}`)
    
}
const resetCounter =()=>{
    totalCount.value=0;
    addMessage("计数器已重置")

}

const addMessage = (text) =>{
    const now=new Date();
    const time = now.toLocaleTimeString();
    messages.value.unshift({
        id:messageId.value++,
        time,
        text
    })
    if(messages.value.length >10)
    {
        messages.value = messages.value.slice(0,10)
    }
}

</script>


<style scoped>
 body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #fecfef 100%);
            min-height: 100vh;
        }
        
        .container {
            max-width: 900px;
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
            border: 2px solid #fce4ec;
            border-radius: 10px;
            background: #fafafa;
        }
        
        .exercise h3 {
            color: #c2185b;
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
        
        .counter-container {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            margin: 15px 0;
            box-shadow: 0 8px 25px rgba(0,0,0,0.15);
        }
        
        .counter-display {
            font-size: 3em;
            font-weight: bold;
            margin: 20px 0;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        
        .counter-controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
        }
        
        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .btn-primary {
            background: #4CAF50;
            color: white;
        }
        
        .btn-primary:hover {
            background: #45a049;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(76, 175, 80, 0.4);
        }
        
        .btn-danger {
            background: #f44336;
            color: white;
        }
        
        .btn-danger:hover {
            background: #da190b;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(244, 67, 54, 0.4);
        }
        
        .btn-warning {
            background: #ff9800;
            color: white;
        }
        
        .btn-warning:hover {
            background: #e68900;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(255, 152, 0, 0.4);
        }
        
        .message-board {
            background: #e8f5e8;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            border: 2px solid #4caf50;
        }
        
        .message-board h4 {
            margin: 0 0 15px 0;
            color: #2e7d32;
        }
        
        .message-item {
            background: white;
            padding: 10px 15px;
            margin: 8px 0;
            border-radius: 8px;
            border-left: 4px solid #4caf50;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .message-item .timestamp {
            font-size: 0.8em;
            color: #666;
            float: right;
        }
        
        .todo-container {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            border: 2px solid #6c757d;
        }
        
        .todo-input {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .todo-input input {
            flex: 1;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
        }
        
        .todo-input input:focus {
            outline: none;
            border-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
        }
        
        .todo-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        
        .todo-item {
            background: white;
            padding: 15px;
            margin: 10px 0;
            border-radius: 8px;
            border: 1px solid #dee2e6;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all 0.3s ease;
        }
        
        .todo-item:hover {
            transform: translateX(5px);
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        
        .todo-item.completed {
            opacity: 0.6;
            text-decoration: line-through;
        }
        
        .todo-actions {
            display: flex;
            gap: 8px;
        }
        
        .btn-sm {
            padding: 6px 12px;
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