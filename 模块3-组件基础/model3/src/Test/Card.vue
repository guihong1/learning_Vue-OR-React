只推进到了用户信息卡面

<template>
<div class="app-container" id="app">
    <div class="control-panel">
        <h3 style="color: white; margin-bottom:15px">🎛️ 组件控制面板</h3>
        <div class="control-group">
            <label >显示模式</label>
            <select v-model="displayMode">
                <option value="grid">网格布局</option>
                <option value="list">列表布局</option>
            </select>
            <label >卡片主题</label>
            <select v-model="cardTheme">
                <option value="default">默认主题</option>
                <option value="dark">深色主题</option>
                <option value="colorful">彩色主题</option>
            </select>

            <button @click="addRandomCard" class="btn">添加随机卡片</button>
            <button @click="clearAllCards" class="btn btn-danger">清空所有卡片</button>
        </div>
    </div>
    <!-- 动态组件演示 -->
    <div class='tab-container'>
        <h3 style="color : white; margin-bottom:15px">🔄 动态组件演示</h3>
        <div class="tab-buttons">
            <button v-for="tab in tabs" :key="tab.name" @click="currentTab = tab.name"
            :class="['tab-button',{active:currentTab === tab.name}]"
            >
            {{tab.label}}
            </button>
        </div>
        <!-- 动态页面切换 -->
         <div class="tab-content">
            <Transition name="fade" mode="out-in">
                <component :is="currentTabComponent" :data="currentTabData"></component>
            </Transition>
         </div>
    </div>
        <!-- 基础卡片展示 -->
         <div class="section">
            <h2 class="section-title">📋 基础卡片组件</h2>
            <div :class="displayMode === 'grid' ? 'card-grid' : 'card-list'">
                <base-card
                    v-for="card in baseCards"
                    :key="card.id"
                    :title="card.title"
                    :content="card.content"
                    :theme="cardTheme"
                    @card-click="handleCardClick"
                    @card-delete="deleteCard">
                    <template #header>
                        <span>
                            <strong>{{card.title}}</strong>
                        </span>
                    </template>

                    <template #footer>
                        <button @click="editCard(card)" class="btn btn-secondary">编辑</button>
                        <button @click="deleteCard(card.id,'base')" class="btn btn-danger">删除</button>
                    </template>
                </base-card> 
            </div>
         </div>
         <!-- 用户卡片展示 -->
          <div class="section">
            <h2 class="section-title">👤 用户卡片信息</h2>
            <div :class="displayMode === 'grid'? 'card-grid' : 'card-list'">
                <user-card
                v-for="user in users"
                :key="user.id"
                :user="user"
                :theme="cardTheme"
                @follow="followUser"
                @message="messageUser"
                @delete="deleteCard"
                ></user-card>
            </div>
          </div>

</div>

</template>


<script setup>
import {ref} from 'vue'
const displayMode = ref('grid')
const cardTheme = ref('default')
const message=ref('')
const messageType = ref('success')
const currentTab = ref('user')
const tabs = ref([
    {
        name:'users',label:'用户管理'
    },
    {
        name:'products',label:'产品管理'
    },
    {
        name:'analytics',label:'数据统计'
    }
])

const baseCards = ref([
    {
        id:1,
        title:'欢迎使用组件库',
        content:'这是一个基础卡片组件，展示了插槽的使用方法'
    },
    {
        id:2,
        title: '组件通信演示',
        content: '点击卡片可以触发父子组件通信事件。'
    }
])

const users = ref([
     {
        id: 1,
        name: '张三',
        role: '前端开发工程师',
        bio: '热爱Vue.js开发，专注于用户体验设计。',
        avatar: 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=150&h=150&fit=crop&crop=face',
        followers: 1234,
        following: 567,
        posts: 89
    },
    {
        id: 2,
        name: '李四',
        role: 'UI/UX 设计师',
        bio: '专注于创造美观且实用的用户界面。',
        avatar: 'https://images.unsplash.com/photo-1494790108755-2616b612b786?w=150&h=150&fit=crop&crop=face',
        followers: 2345,
        following: 123,
        posts:156
    }

])

const products = ref([
    {
        id: 1,
        name: 'MacBook Pro 16"',
        description: '强大的专业级笔记本电脑，适合开发和设计工作。',
        price: 19999,
        originalPrice: 21999,
        stock: 15,
        image: 'https://images.unsplash.com/photo-1517336714731-489689fd1ca8?w=300&h=200&fit=crop',
        badge: '热销'
    },
    {
        id: 2,
        name: 'iPhone 15 Pro',
        description: '最新的iPhone，配备强大的A17芯片和专业级摄像系统。',
        price: 8999,
        originalPrice: null,
        stock: 8,
        image: 'https://images.unsplash.com/photo-1592750475338-74b7b21085ab?w=300&h=200&fit=crop',
        badge: '新品'
    }
])

const statistics = ref([
    {
        id: 1,
        title: '总用户数',
        value: 12567,
        change: 12.5,
        icon: '👥'
    },
    {
        id: 2,
        title: '月收入',
        value: 156789,
        change: -3.2,
        icon: '💰'
    },
    {
        id: 3,
        title: '活跃用户',
        value: 8934,
        change: 8.7,
        icon: '🔥'
    }
])

const currentTabComponent = computed(()=>{
    const componentMap = {
        users: 'UserManagement',
        products: 'ProductManagement',
        analytics: 'DataAnalytics'
    }
    return componentMap[currentTab.value]
})
const currentTabData = computed (()=>{
    const dataMap = {
        users: {
                userCount: users.value.length,
            },
        products: {
            productCount: products.value.length,
            totalValue: products.value.reduce((sum, p) => sum + (p.price * p.stock), 0)
        },
        analytics: {
            todayVisits: 3456,
            totalRevenue: 234567,
            activeUsers: statistics.value.find(s => s.title === '活跃用户')?.value || 0
        }
    }
    return dataMap[currentTab.value]
})

const showMessage = (text,type = 'success')=>{
    message.value = text;
    messageType.value = type;
    setTimeout(()=>{
        message.value = '';
    },2000)

}

const addRandomCard=() =>{
    const types = ['base','user','product','stat']
    const type = types[Math.floor(Math.random()* types.length)]

    switch (type){
        case 'base':
            baseCards.value.push({
                id:Date.now(),
                title:`随机卡片 ${baseCards.value.length +1}`,
                content:`这是一个随机生成的卡片内容`
            });
            break;
        case 'user':
            users.value.push({
                id: Date.now(),
                name: `用户${users.value.length + 1}`,
                role: '新用户',
                bio: '这是一个新注册的用户。',
                avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?w=150&h=150&fit=crop&crop=face',
                followers: Math.floor(Math.random() * 1000),
                following: Math.floor(Math.random() * 500),
                posts: Math.floor(Math.random() * 100)
            });
            break;

        case 'product':
            products.value.push({
                id: Date.now(),
                name: `产品${products.value.length + 1}`,
                description: '这是一个新的产品。',
                price: Math.floor(Math.random() * 10000) + 1000,
                originalPrice: null,
                stock: Math.floor(Math.random() * 50) + 1,
                image: 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=300&h=200&fit=crop',
                badge: '新品'
            });
            break;
            case 'stat':
            statistics.value.push({
            id: Date.now(),
            title: `统计${statistics.value.length + 1}`,
            value: Math.floor(Math.random() * 100000),
            change: (Math.random() - 0.5) * 20,
            icon: '📈'
            });
            break;
            
    }
    showMessage(`添加了一个随机${type}卡片`,`success`)
}
const clearAllCards = ()=>{
    if(confirm('确定要清空所有卡片吗？')){
        baseCards.value=[];
        users.value=[];
        products.value=[];
        statistics.value=[];
        showMessage('所有卡片已清空','success')
    }

}

const handleCardClick = cardData => {
    showMessage(`点击了卡片:${cardData.title}`,'success')
}

const editCard = card =>{
    showMessage(`编辑了卡片:${card.title}`,'info')
}

const deleteCard = (id,type) => {
    const collections = {
        base : baseCards.value,
        user : users.value,
        product : products.value,
        stat : statistics.value
    }
    const collection = collections[type]
    if(collection){
        const index = collection.findIndex(item =>item.id === id)
        if(index !== -1){
            collection.splice(index,1)
            showMessage(`删除了${type}卡片${id}`,'success')
        }

    }
}

const followUser = userId => {
    showMessage(`关注了用户 ${userId}`,'success')
}
const messageUser = userId => {
    showMessage(`给用户 ${userId} 发送了一条消息`,'info')
}

</script>



<style scoped>


</style>