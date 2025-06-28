# 模块1：Vue.js基础概念与环境搭建

## 🎯 学习目标
- 理解Vue.js的核心概念和特点
- 搭建Vue.js开发环境
- 创建第一个Vue应用
- 掌握Vue实例和生命周期
- 学会基本的模板语法

## 📚 理论知识

### 1. Vue.js简介

Vue.js是一个用于构建用户界面的渐进式JavaScript框架。它的核心特点包括：

- **渐进式**：可以逐步采用，不需要重写整个应用
- **响应式**：数据变化时，视图自动更新
- **组件化**：将界面拆分成可复用的组件
- **轻量级**：核心库只关注视图层
- **易学易用**：学习曲线平缓，上手容易

### 2. MVVM架构模式

Vue.js采用MVVM（Model-View-ViewModel）架构：
- **Model**：数据模型
- **View**：视图层（DOM）
- **ViewModel**：Vue实例，连接Model和View

### 3. Vue实例生命周期

Vue实例从创建到销毁会经历以下阶段：
1. **beforeCreate**：实例初始化之后，数据观测之前
2. **created**：实例创建完成，数据观测已完成
3. **beforeMount**：挂载开始之前
4. **mounted**：实例挂载完成
5. **beforeUpdate**：数据更新时调用
6. **updated**：数据更新完成后调用
7. **beforeDestroy**：实例销毁之前
8. **destroyed**：实例销毁后调用

## 🛠️ 环境搭建

### 步骤1：安装Node.js
1. 访问 https://nodejs.org/
2. 下载LTS版本
3. 安装完成后，打开命令行验证：
```bash
node --version
npm --version
```

### 步骤2：安装Vue CLI
```bash
npm install -g @vue/cli
# 或者使用yarn
yarn global add @vue/cli
```

### 步骤3：创建Vue项目
```bash
vue create my-first-vue-app
```

## 💻 实践代码

### 1. 基础HTML引入方式

创建一个简单的HTML文件来体验Vue：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的第一个Vue应用</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
        <p>当前时间：{{ currentTime }}</p>
        <button @click="updateTime">更新时间</button>
    </div>

    <script>
        const { createApp } = Vue;
        
        createApp({
            data() {
                return {
                    message: '欢迎学习Vue.js！',
                    currentTime: new Date().toLocaleString()
                }
            },
            methods: {
                updateTime() {
                    this.currentTime = new Date().toLocaleString();
                }
            },
            mounted() {
                console.log('Vue应用已挂载！');
            }
        }).mount('#app');
    </script>
</body>
</html>
```

### 2. 生命周期演示

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue生命周期演示</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>
    <div id="lifecycle-demo">
        <h2>生命周期演示</h2>
        <p>计数器：{{ count }}</p>
        <button @click="increment">增加</button>
        <button @click="destroy">销毁组件</button>
    </div>

    <script>
        const { createApp } = Vue;
        
        const app = createApp({
            data() {
                return {
                    count: 0
                }
            },
            methods: {
                increment() {
                    this.count++;
                },
                destroy() {
                    this.$destroy();
                }
            },
            // 生命周期钩子
            beforeCreate() {
                console.log('1. beforeCreate - 实例初始化之后，数据观测之前');
            },
            created() {
                console.log('2. created - 实例创建完成，数据观测已完成');
            },
            beforeMount() {
                console.log('3. beforeMount - 挂载开始之前');
            },
            mounted() {
                console.log('4. mounted - 实例挂载完成');
            },
            beforeUpdate() {
                console.log('5. beforeUpdate - 数据更新时调用');
            },
            updated() {
                console.log('6. updated - 数据更新完成后调用');
            },
            beforeUnmount() {
                console.log('7. beforeUnmount - 实例销毁之前');
            },
            unmounted() {
                console.log('8. unmounted - 实例销毁后调用');
            }
        });
        
        app.mount('#lifecycle-demo');
    </script>
</body>
</html>
```

## 🎯 实践项目：计数器应用

创建一个功能完整的计数器应用，包含以下功能：
- 显示当前计数
- 增加/减少按钮
- 重置功能
- 步长设置
- 历史记录

## 📝 作业任务

1. **环境搭建**：按照步骤搭建Vue开发环境
2. **基础练习**：运行提供的示例代码，观察控制台输出
3. **实践项目**：完成计数器应用的开发
4. **思考题**：
   - Vue.js相比原生JavaScript有什么优势？
   - MVVM模式是如何工作的？
   - 生命周期钩子的作用是什么？

## 🔍 知识点检查

完成本模块后，你应该能够：
- [ ] 解释Vue.js的核心概念
- [ ] 搭建Vue开发环境
- [ ] 创建基本的Vue应用
- [ ] 理解Vue实例生命周期
- [ ] 使用基本的模板语法
- [ ] 处理用户交互事件

## 📖 扩展阅读

- [Vue.js官方文档 - 介绍](https://vuejs.org/guide/introduction.html)
- [Vue.js官方文档 - 生命周期](https://vuejs.org/guide/essentials/lifecycle.html)
- [Vue CLI官方文档](https://cli.vuejs.org/)

---

**准备好了吗？让我们开始编写你的第一个Vue应用！**

有任何问题随时问我，我会详细解答每一个知识点。