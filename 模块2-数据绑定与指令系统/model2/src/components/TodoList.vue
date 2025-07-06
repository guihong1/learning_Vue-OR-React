<template>
  <div id="todo-app">
    <!-- Header -->
    <div class="header">
      <h1>📝 Vue Todo List</h1>
      <p>使用Vue.js数据绑定与指令系统构建的任务管理应用</p>
    </div>

    <!-- Input Section -->
    <div class="input-section">
      <div class="input-container">
        <input
          v-model="newTask.text"
          @keyup.enter="addTask"
          class="task-input"
          placeholder="输入任务..."
          :disabled="isLoading"
        />
        <select v-model="newTask.priority" class="priority-select">
          <option value="low">低优先级</option>
          <option value="medium">中优先级</option>
          <option value="high">高优先级</option>
        </select>
        <button
          @click="addTask"
          :disabled="!newTask.text.trim() || isLoading"
          class="add-btn"
        >
          {{ isLoading ? '添加中...' : '添加任务' }}
        </button>
      </div>

      <!-- Search -->
      <div class="search-container">
        <input
          v-model="searchQuery"
          placeholder="搜索任务..."
          class="search-input"
        />
      </div>

      <!-- Progress Bar -->
      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
      </div>
    </div>

    <!-- Filter and Stats -->
    <div class="controls">
      <!-- Filter Buttons -->
      <div class="filter-buttons">
        <button
          v-for="filter in filters"
          :key="filter.key"
          @click="currentFilter = filter.key"
          :class="['filter-btn', { active: currentFilter === filter.key }]"
        >
          {{ filter.label }}({{ getFilterCount(filter.key) }})
        </button>
      </div>

      <!-- Stats -->
      <div class="stats">
        <div class="stat-item">
          总计：<span class="stat-number">{{ todos.length }}</span>
        </div>
        <div class="stat-item">
          已完成：<span class="stat-number">{{ completedCount }}</span>
        </div>
        <div class="stat-item">
          进度：<span class="stat-number">{{ progressPercentage }}</span>%
        </div>
      </div>
    </div>

    <!-- Todo List -->
    <div class="todo-list">
      <!-- Empty State -->
      <div v-if="filteredTodos.length === 0" class="empty-state">
        <div class="icon">{{ getEmptyStateIcon() }}</div>
        <h3>{{ getEmptyStateMessage() }}</h3>
        <p v-if="currentFilter === 'all' && searchQuery === ''">开始添加你的第一个任务吧！</p>
      </div>

      <!-- Task Items -->
      <div
        v-for="(todo, index) in filteredTodos"
        :key="todo.id"
        :class="[
          'todo-item',
          { completed: todo.completed },
          todo.priority + '-priority'
        ]"
      >
        <!-- Checkbox -->
        <input
          type="checkbox"
          v-model="todo.completed"
          class="todo-checkbox"
          @change="updateProgress"
        />

        <!-- Content -->
        <div class="todo-content">
          <div
            :class="['todo-text', { completed: todo.completed, editing: todo.editing }]"
            @dblclick="startEdit(todo)"
          >
            {{ todo.text }}
          </div>

          <!-- Edit Input -->
          <input
            v-if="todo.editing"
            v-model="todo.editText"
            @keyup.enter="finishEdit(todo)"
            @keyup.esc="cancelEdit(todo)"
            @blur="finishEdit(todo)"
            class="todo-edit active"
            ref="editInput"
          />

          <!-- Meta Info -->
          <div class="todo-meta">
            <span :class="['priority-badge', 'priority-' + todo.priority]">
              {{ getPriorityText(todo.priority) }}
            </span>
            <span>创建时间：{{ formatDate(todo.createdAt) }}</span>
            <span v-if="todo.completedAt">完成时间：{{ formatDate(todo.completedAt) }}</span>
          </div>
        </div>

        <!-- Actions -->
        <div class="todo-actions">
          <button
            @click="startEdit(todo)"
            class="action-btn edit-btn"
            :disabled="todo.completed"
          >
            编辑
          </button>
          <button @click="deleteTask(index)" class="action-btn delete-btn">删除</button>
        </div>
      </div>
    </div>

    <!-- Bulk Actions -->
    <div class="bulk-actions" v-if="todos.length > 0">
      <button @click="toggleAll" class="bulk-btn">
        {{ allCompleted ? '全部取消' : '全部完成' }}
      </button>
      <button @click="clearCompleted" class="bulk-btn" :disabled="completedCount === 0">
        清除已完成 ({{ completedCount }})
      </button>
      <button @click="clearAll" class="bulk-btn">清空所有</button>
      <button @click="exportTasks" class="bulk-btn">导出任务</button>
    </div>
  </div>
</template>


<script setup>
import {ref,computed,nextTick} from 'vue'
const newTask = ref({
    text: '',
    priority:'medium'
})

const todos= ref([
   {
    id: 1,
    text: '学习Vue.js数据绑定',
    completed: false,
    priority: 'high',
    createdAt: new Date(),
    completedAt: null,
    editing: false,
    editText: ''
    },
    {
    id: 2,
    text: '掌握Vue指令系统',
    completed: false,
    priority: 'medium',
    createdAt: new Date(Date.now() - 3600000),
    completedAt: null,
    editing: false,
    editText: ''
    },
    {
    id: 3,
    text: '完成Todo List项目',
    completed: true,
    priority: 'low',
    createdAt: new Date(Date.now() - 7200000),
    completedAt: new Date(Date.now() - 1800000),
    editing: false,
    editText: ''
    }
])

const currentFilter = ref('all')
const filters = ref([
    {key:'all',label:'全部'},
    {key:'active',label:'未完成'},
    {key:'completed',label:'已完成'}
])
const searchQuery = ref('')
const isLoading = ref(false)
const nextId=ref(4)

const filteredTodos = computed(() => {
  let result = todos.value;

  // 按状态过滤
  if (currentFilter.value === 'active') {
    result = result.filter(todo => !todo.completed);
  } else if (currentFilter.value === 'completed') {
    result = result.filter(todo => todo.completed);
  }

  // 按搜索关键词过滤
  const query = searchQuery.value.trim().toLowerCase();
  if (query) {
    result = result.filter(todo => todo.text.toLowerCase().includes(query));
  }

  // 按优先级排序
  const priorityOrder = { high: 3, medium: 2, low: 1 };
  return result.sort((a, b) => priorityOrder[b.priority] - priorityOrder[a.priority]);
});

const completedCount = computed(() => {
  return todos.value.filter(todo => todo.completed).length;
});

const progressPercentage = computed (()=>{
    if(todos.value.length === 0) return 0
    return Math.round(completedCount.value / todos.value.length) * 100
})

const allCompleted = computed (()=>{
    return todos.value.length !== 0 && todos.value.length === completedCount.value
})

function getFilterCount(filterKey) {
  if (filterKey === 'all') {
    return todos.value.length;
  }
  if(filterKey === 'active')
  return todos.value.length - completedCount.value
  if(filterKey === 'completed')
  return completedCount.value
  return 0;
}

function getEmptyStateIcon()
{
    if(searchQuery.value.trim()) return '🔍';
    if(currentFilter.value === 'completed') return '🎉';
    if(currentFilter.value === 'active') return '💪';
    return '📝';
}

function getEmptyStateMessage(){
    if(searchQuery.value.trim()) return '没有找到匹配的任务'
    if(currentFilter.value === 'completed') return '没有已完成的任务'
    if(allCompleted.value) return '所有任务已完成'
    return '暂无任务'
}


function startEdit(todo) {
  if (todo.completed) return

  // 设置编辑状态和临时文本
  todo.editing = true
  todo.editText = todo.text

  // 确保 DOM 更新后再聚焦输入框
  nextTick(() => {
    const inputEl = editInput.value

    if (Array.isArray(inputEl)) {
      // 如果有多个 input（v-for），取最后一个
      const lastInput = inputEl[inputEl.length - 1]
      lastInput.focus()
      lastInput.select()
    } else if (inputEl) {
      // 单个 input
      inputEl.focus()
      inputEl.select()
    }
  })
}
function finishEdit(todo) {
    if(!todo.editing) return;
    const newText=todo.editText.trim();
    if(newText)
    {
        todo.text = newText;
    }
        todo.editing = false;
        todo.editText ='';
        saveTodos();  


}

function addTask()
{
  if(!newTask.value.text.trim()) return ;
  isLoading.value=true;
  setTimeout(()=>{
    const task = {
      id:nextId.value,
      text:newTask.value.text,
      completed:false,
      priority:newTask.value.priority,
      createdAt:new Date(),
      completedAt:null,
      editing:false,
      editText:''
    }
    todos.value.push(task);
    newTask.value.text = '';
    newTask.value.priority = 'medium';
    nextId.value++;
    saveTodos();
    isLoading.value=false;
  },1000)

}
function deleteTask(index){
  if(confirm('确认删除吗？'))
  {
    const todo =filteredTodos.value[index];
    const originalIndex = todos.value.findIndex(t =>t.id === todo.id);
    todos.value.splice(originalIndex,1);
    saveTodos();
  }
}
function cancelEdit(todo){
  todo.editing = false;
  todo.editText = '';
}

function toggleAll(){
  const targetState = ! allCompleted.value;
  todos.value.forEach(todo => {
    todo.completed = targetState;
    todo.completedAt = targetState ? new Date() : null;
  
});
  saveTodos();
}

function clearCompleted()
{
  if(confirm(`确定要删除${completedCount.value}条已完成任务吗？`))
  {
    todos.value = todos.value.filter(todo => !todo.completed);
    saveTodos();
  }

}


function clearAll()
{
  if(confirm(`确定要删除所有${todos.value.length}条任务吗？`))
  {
    todos.value = [];
    saveTodos();
  }
}

function getPriorityText(priority)
{
  const map = {
    high:'高优先级',
    medium:'中优先级',
    low:'低优先级'
  }
  return map[priority] || priority
}

function formatDate(date)
{
  if(!date) return '';
  return new Date(date).toLocaleString('zh-CN',{
    month:'short',
    day:'2-digit',
    hour:'2-digit',
    minute:'2-digit'
  })
}


function saveTodos()
{
  try{
    localStorage.setItem('vue-todos',JSON.stringify(todos.value));
  }
  catch(error){
    console.error('数据保存失败',error);
  }
}

function exportTasks()
{
  const data = {
    exportDate: new Date().toISOString(),
    totalTasks: todos.value.length,
    completedTasks: completedCount.value,
    tasks:todos.value.map(todo => ({
      text:todo.text,
      completed:todo.completed,
      priority:todo.priority,
      createdAt:todo.createdAt,
      completedAt:todo.completedAt
    }))
  }

  const blob = new Blob([JSON.stringify(data,null,2)],{type: 'application/json'})
  const url = URL.createObjectURL(blob);
  const a =document.createElement('a');
  a.href = url;
  a.download = `todos-${new Date().toISOString().split('T')[0]}.json`;
  a.click();
  URL.revokeObjectURL(url);
}

function updateProgress(){
  todos.value.forEach(todo =>{
    if(todo.completed && !todo.completedAt)
    {
      todo.completedAt = new Date();
    }
    else if(!todo.completed && todo.completedAt)
    {
      todo.completedAt = null;
    }

  })
  saveTodos()
}

function loadTodos()
{
  
}




</script>


<style scoped>


* {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .todo-app {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }
        
        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }
        
        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
        }
        
        .header p {
            opacity: 0.9;
            font-size: 1.1em;
        }
        
        .input-section {
            padding: 30px;
            border-bottom: 1px solid #eee;
        }
        
        .input-container {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .task-input {
            flex: 1;
            padding: 15px 20px;
            border: 2px solid #e1e8ed;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s ease;
        }
        
        .task-input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }
        
        .add-btn {
            padding: 15px 30px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .add-btn:hover {
            background: #5a6fd8;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }
        
        .add-btn:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .priority-select {
            padding: 15px;
            border: 2px solid #e1e8ed;
            border-radius: 10px;
            font-size: 16px;
            background: white;
        }
        
        .controls {
            padding: 20px 30px;
            background: #f8f9fa;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .filter-buttons {
            display: flex;
            gap: 10px;
        }
        
        .filter-btn {
            padding: 8px 16px;
            border: 2px solid #667eea;
            background: white;
            color: #667eea;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
        }
        
        .filter-btn.active {
            background: #667eea;
            color: white;
        }
        
        .filter-btn:hover {
            background: #667eea;
            color: white;
        }
        
        .stats {
            display: flex;
            gap: 20px;
            align-items: center;
        }
        
        .stat-item {
            font-weight: bold;
            color: #333;
        }
        
        .stat-number {
            color: #667eea;
            font-size: 1.2em;
        }
        
        .todo-list {
            padding: 0 30px 30px;
        }
        
        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: #666;
        }
        
        .empty-state .icon {
            font-size: 4em;
            margin-bottom: 20px;
            opacity: 0.5;
        }
        
        .todo-item {
            display: flex;
            align-items: center;
            padding: 20px;
            margin-bottom: 15px;
            background: #f8f9fa;
            border-radius: 15px;
            border-left: 5px solid transparent;
            transition: all 0.3s ease;
            position: relative;
        }
        
        .todo-item:hover {
            background: #e9ecef;
            transform: translateX(5px);
        }
        
        .todo-item.completed {
            opacity: 0.7;
            background: #e8f5e8;
            border-left-color: #28a745;
        }
        
        .todo-item.high-priority {
            border-left-color: #dc3545;
        }
        
        .todo-item.medium-priority {
            border-left-color: #ffc107;
        }
        
        .todo-item.low-priority {
            border-left-color: #28a745;
        }
        
        .todo-checkbox {
            width: 20px;
            height: 20px;
            margin-right: 15px;
            cursor: pointer;
        }
        
        .todo-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 5px;
        }
        
        .todo-text {
            font-size: 16px;
            line-height: 1.5;
            transition: all 0.3s ease;
        }
        
        .todo-text.completed {
            text-decoration: line-through;
            color: #666;
        }
        
        .todo-text.editing {
            display: none;
        }
        
        .todo-edit {
            display: none;
            padding: 8px 12px;
            border: 2px solid #667eea;
            border-radius: 5px;
            font-size: 16px;
            width: 100%;
        }
        
        .todo-edit.active {
            display: block;
        }
        
        .todo-meta {
            display: flex;
            gap: 15px;
            font-size: 12px;
            color: #666;
        }
        
        .priority-badge {
            padding: 2px 8px;
            border-radius: 10px;
            font-weight: bold;
            font-size: 10px;
            text-transform: uppercase;
        }
        
        .priority-high {
            background: #ffe6e6;
            color: #dc3545;
        }
        
        .priority-medium {
            background: #fff3cd;
            color: #856404;
        }
        
        .priority-low {
            background: #d4edda;
            color: #155724;
        }
        
        .todo-actions {
            display: flex;
            gap: 10px;
        }
        
        .action-btn {
            padding: 8px 12px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 12px;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        
        .edit-btn {
            background: #17a2b8;
            color: white;
        }
        
        .edit-btn:hover {
            background: #138496;
        }
        
        .delete-btn {
            background: #dc3545;
            color: white;
        }
        
        .delete-btn:hover {
            background: #c82333;
        }
        
        .bulk-actions {
            padding: 20px 30px;
            background: #f8f9fa;
            border-top: 1px solid #eee;
            display: flex;
            gap: 15px;
            justify-content: center;
        }
        
        .bulk-btn {
            padding: 10px 20px;
            border: 2px solid #6c757d;
            background: white;
            color: #6c757d;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        
        .bulk-btn:hover {
            background: #6c757d;
            color: white;
        }
        
        .search-container {
            margin-bottom: 20px;
        }
        
        .search-input {
            width: 100%;
            padding: 12px 20px;
            border: 2px solid #e1e8ed;
            border-radius: 10px;
            font-size: 14px;
        }
        
        .search-input:focus {
            outline: none;
            border-color: #667eea;
        }
        
        .progress-bar {
            margin-top: 15px;
            height: 8px;
            background: #e9ecef;
            border-radius: 4px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            transition: width 0.3s ease;
        }
        
        @media (max-width: 768px) {
            .controls {
                flex-direction: column;
                align-items: stretch;
            }
            
            .filter-buttons {
                justify-content: center;
            }
            
            .stats {
                justify-content: center;
            }
            
            .bulk-actions {
                flex-wrap: wrap;
            }
        }

</style>
