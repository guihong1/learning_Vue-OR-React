<template>
<div class="container">
    <div class="exercise">
        <div class="demo-area" id="app">
            <div class="controls">
                <button @click="showModal = true" class="btn btn-primary">打开模态框</button>
                <button @click="showConfirm = true" class="btn btn-success">确认对话框</button>
                <button @click="showForm = true" class="btn btn-secondary">表单对话框</button>
            </div>
            <modal :show="showModal" @close="showModal = false">
                <template #header>
                    <h3>欢迎使用vue3</h3>
                </template>

                <template #default>
                    <p>这是一个使用具名插槽的模态框组件</p>
                    <p>你可以在不同的插槽中放置不同的内容：</p>
                    <ul>
                        <li>header插槽:放置标题</li>
                        <li>default插槽:放置主要内容</li>
                        <li>footer插槽:放置底部操作</li>
                    </ul>
                </template>

                 <template #footer>
                    <button @click="showModal = false" class="btn btn-secondary">取消</button>
                    <button @click="showModal = false" class="btn btn-primary">确定</button>
                </template>
            </modal>
                     <!-- 确认对话框 -->
            <modal :show="showConfirm" @close="showConfirm = false">
                <template #header>
                    <h3>⚠️ 确认操作</h3>
                </template>
                    
                 <template #default>
                    <p>你确定要执行这个操作吗？此操作不可撤销。</p>
                </template>
                    
                <template #footer>
                    <button @click="showConfirm = false" class="btn btn-secondary">取消</button>
                    <button @click="confirmAction" class="btn btn-danger">确认删除</button>
                </template>
            </modal>


        <!-- 表单对话框 -->
        <modal :show="showForm" @close="showForm = false">
            <template #header>
                <h3>📝 添加用户</h3>
            </template>
                    
            <template #default>
                <div style="margin: 15px 0;">
                    <label style="display: block; margin-bottom: 5px; font-weight: bold;">姓名：</label>
                    <input v-model="formData.name" style="width: 100%; padding: 8px; border: 1px solid #ddd; border-radius: 4px;">
                </div>
                <div style="margin: 15px 0;">
                    <label style="display: block; margin-bottom: 5px; font-weight: bold;">邮箱：</label>
                    <input v-model="formData.email" type="email" style="width: 100%; padding: 8px; border: 1px solid #ddd; border-radius: 4px;">
                </div>
                <div style="margin: 15px 0;">
                    <label style="display: block; margin-bottom: 5px; font-weight: bold;">角色：</label>
                    <select v-model="formData.role" style="width: 100%; padding: 8px; border: 1px solid #ddd; border-radius: 4px;">
                        <option value="user">普通用户</option>
                        <option value="admin">管理员</option>
                        <option value="editor">编辑者</option>
                    </select>
                </div>
            </template>
                    
            <template #footer>
                <button @click="showForm = false" class="btn btn-secondary">取消</button>
                <button @click="submitForm" class="btn btn-success">保存</button>
            </template>
        </modal>

    </div>

    </div>
</div>

</template>


<script setup>
import {ref} from 'vue'
import Modal from '../components/modal.vue'

const showModal =ref(false)
const showConfirm = ref(false)
const showForm = ref(false)
const formData=ref({
    name:'',
    email:'',
    role:'user'
})
const users = ref([
    { id: 1, name: '张三', email: 'zhangsan@example.com', role: '管理员', status: 'active' },
    { id: 2, name: '李四', email: 'lisi@example.com', role: '编辑者', status: 'active' },
    { id: 3, name: '王五', email: 'wangwu@example.com', role: '用户', status: 'inactive' },
    { id: 4, name: '赵六', email: 'zhaoliu@example.com', role: '用户', status: 'pending' }
                    
])

const confirmAction= ()=>{
    if(formData.value.name && formData.value.email){
        users.push({
            id:users.value.length+1,
            name:formData.value.name,
            email:formData.value.email,
            role:formData.value.role,
            status:'active'
        })
        formData.value.name=''
        formData.value.email=''
        formData.value.role='user'
        showForm.value=false
        alert('用户添加成功')
    }else {
        alert('请输入姓名和邮箱')
    }
}

</script>


<style scoped>
body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            min-height: 100vh;
        }
        
        .container {
            max-width: 1000px;
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
            border: 2px solid #e0f2f1;
            border-radius: 10px;
            background: #fafafa;
        }
        
        .exercise h3 {
            color: #00695c;
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
        
        /* 模态框样式 */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .modal-container {
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            max-width: 500px;
            width: 90%;
            max-height: 80vh;
            overflow: hidden;
            animation: modalSlideIn 0.3s ease;
        }
        
        @keyframes modalSlideIn {
            from {
                opacity: 0;
                transform: translateY(-50px) scale(0.9);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }
        
        .modal-header {
            padding: 20px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        
        .modal-header h3 {
            margin: 0;
            font-size: 1.5em;
        }
        
        .modal-close {
            background: none;
            border: none;
            color: white;
            font-size: 24px;
            cursor: pointer;
            padding: 0;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background 0.3s;
        }
        
        .modal-close:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        .modal-body {
            padding: 20px;
            max-height: 400px;
            overflow-y: auto;
        }
        
        .modal-footer {
            padding: 15px 20px;
            border-top: 1px solid #eee;
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            background: #f8f9fa;
        }
        
        /* 卡片布局样式 */
        .layout-card {
            border: 1px solid #ddd;
            border-radius: 10px;
            overflow: hidden;
            margin: 15px 0;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            transition: transform 0.3s ease;
        }
        
        .layout-card:hover {
            transform: translateY(-5px);
        }
        
        .layout-header {
            background: linear-gradient(135deg, #4CAF50 0%, #45a049 100%);
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        .layout-header h3 {
            margin: 0;
            font-size: 1.5em;
        }
        
        .layout-sidebar {
            background: #f8f9fa;
            padding: 20px;
            border-right: 1px solid #dee2e6;
        }
        
        .layout-content {
            padding: 20px;
            flex: 1;
        }
        
        .layout-footer {
            background: #343a40;
            color: white;
            padding: 15px 20px;
            text-align: center;
        }
        
        .layout-body {
            display: flex;
            min-height: 200px;
        }
        
        /* 数据表格样式 */
        .data-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        
        .data-table th {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px;
            text-align: left;
            font-weight: bold;
        }
        
        .data-table td {
            padding: 12px 15px;
            border-bottom: 1px solid #eee;
        }
        
        .data-table tr:hover {
            background: #f8f9fa;
        }
        
        .data-table tr:last-child td {
            border-bottom: none;
        }
        
        /* 按钮样式 */
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 6px;
            font-size: 14px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-block;
            text-align: center;
        }
        
        .btn-primary {
            background: #007bff;
            color: white;
        }
        
        .btn-primary:hover {
            background: #0056b3;
            transform: translateY(-2px);
        }
        
        .btn-success {
            background: #28a745;
            color: white;
        }
        
        .btn-success:hover {
            background: #1e7e34;
            transform: translateY(-2px);
        }
        
        .btn-danger {
            background: #dc3545;
            color: white;
        }
        
        .btn-danger:hover {
            background: #c82333;
            transform: translateY(-2px);
        }
        
        .btn-secondary {
            background: #6c757d;
            color: white;
        }
        
        .btn-secondary:hover {
            background: #545b62;
            transform: translateY(-2px);
        }
        
        /* 控制面板 */
        .controls {
            background: #e8f5e8;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            align-items: center;
        }
        
        .controls label {
            font-weight: bold;
            color: #2e7d32;
        }
        
        .controls select {
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
        }
        
        /* 提示样式 */
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
        
        /* 用户头像 */
        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #007bff;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-right: 10px;
        }
        
        .user-info {
            display: flex;
            align-items: center;
        }
        
        .status-badge {
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
            text-transform: uppercase;
        }
        
        .status-active {
            background: #d4edda;
            color: #155724;
        }
        
        .status-inactive {
            background: #f8d7da;
            color: #721c24;
        }
        
        .status-pending {
            background: #fff3cd;
            color: #856404;
        }
</style>