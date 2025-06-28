# 模块2：数据绑定与指令系统

## 🎯 学习目标
- 深入理解Vue的数据绑定机制
- 掌握双向数据绑定（v-model）的使用
- 熟练运用条件渲染指令（v-if、v-show）
- 掌握列表渲染指令（v-for）的各种用法
- 学会事件处理指令（v-on）的高级用法
- 理解属性绑定指令（v-bind）的应用场景
- 完成Todo List应用项目

## 📚 理论知识

### 1. 数据绑定概述

Vue.js的核心特性之一就是响应式数据绑定。数据绑定让我们可以轻松地将数据与DOM元素连接起来。

#### 数据绑定的类型：
1. **单向绑定**：数据 → 视图
2. **双向绑定**：数据 ↔ 视图
3. **一次性绑定**：数据 → 视图（仅一次）

### 2. 插值表达式详解

#### 基础插值
```html
<!-- 文本插值 -->
<p>{{ message }}</p>

<!-- HTML插值（v-html） -->
<div v-html="rawHtml"></div>

<!-- 属性插值（v-bind） -->
<img v-bind:src="imageSrc">

<!-- 一次性插值（v-once） -->
<p v-once>{{ message }}</p>
```

#### 表达式支持
```html
<!-- 数学运算 -->
{{ number + 1 }}

<!-- 字符串操作 -->
{{ message.split('').reverse().join('') }}

<!-- 三元运算符 -->
{{ ok ? 'YES' : 'NO' }}

<!-- 函数调用 -->
{{ message.toUpperCase() }}
```

### 3. 指令系统详解

#### v-model：双向数据绑定

**基础用法：**
```html
<!-- 文本输入 -->
<input v-model="message" placeholder="输入文本">

<!-- 多行文本 -->
<textarea v-model="message"></textarea>

<!-- 复选框 -->
<input type="checkbox" v-model="checked">

<!-- 单选按钮 -->
<input type="radio" v-model="picked" value="a">
<input type="radio" v-model="picked" value="b">

<!-- 选择框 -->
<select v-model="selected">
  <option disabled value="">请选择</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
```

**修饰符：**
```html
<!-- .lazy：在change事件而非input事件中更新 -->
<input v-model.lazy="msg">

<!-- .number：自动将用户输入转为数值类型 -->
<input v-model.number="age" type="number">

<!-- .trim：自动过滤用户输入的首尾空白字符 -->
<input v-model.trim="msg">
```

#### v-if / v-else-if / v-else：条件渲染

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

**template元素：**
```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

#### v-show：条件显示

```html
<h1 v-show="ok">Hello!</h1>
```

**v-if vs v-show：**
- `v-if`：真正的条件渲染，会销毁和重建元素
- `v-show`：只是切换CSS的display属性
- `v-if`有更高的切换开销，`v-show`有更高的初始渲染开销

#### v-for：列表渲染

**遍历数组：**
```html
<ul>
  <li v-for="item in items" :key="item.id">
    {{ item.message }}
  </li>
</ul>

<!-- 带索引 -->
<ul>
  <li v-for="(item, index) in items" :key="index">
    {{ index }} - {{ item.message }}
  </li>
</ul>
```

**遍历对象：**
```html
<ul>
  <li v-for="value in object" :key="value">
    {{ value }}
  </li>
</ul>

<!-- 带键名 -->
<ul>
  <li v-for="(value, name) in object" :key="name">
    {{ name }}: {{ value }}
  </li>
</ul>

<!-- 带键名和索引 -->
<ul>
  <li v-for="(value, name, index) in object" :key="index">
    {{ index }}. {{ name }}: {{ value }}
  </li>
</ul>
```

**key的重要性：**
```html
<!-- 好的做法 -->
<div v-for="item in items" :key="item.id">
  {{ item.name }}
</div>

<!-- 避免使用索引作为key（在动态列表中） -->
<div v-for="(item, index) in items" :key="index">
  {{ item.name }}
</div>
```

#### v-on：事件处理

**基础用法：**
```html
<!-- 方法处理器 -->
<button v-on:click="doThis">点击</button>

<!-- 内联处理器 -->
<button v-on:click="say('hi')">Say hi</button>

<!-- 缩写 -->
<button @click="doThis">点击</button>
```

**事件修饰符：**
```html
<!-- 阻止单击事件继续传播 -->
<a @click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a @click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form @submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<div @click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<div @click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a @click.once="doThis"></a>
```

**按键修饰符：**
```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input @keyup.enter="submit">

<!-- 常用按键别名 -->
<input @keyup.tab="tabHandler">
<input @keyup.delete="deleteHandler">
<input @keyup.esc="escHandler">
<input @keyup.space="spaceHandler">
<input @keyup.up="upHandler">
<input @keyup.down="downHandler">
<input @keyup.left="leftHandler">
<input @keyup.right="rightHandler">
```

#### v-bind：属性绑定

**基础用法：**
```html
<!-- 绑定属性 -->
<img v-bind:src="imageSrc">

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 动态属性名 -->
<a v-bind:[attributeName]="url"> ... </a>

<!-- 缩写 -->
<a :[attributeName]="url"> ... </a>
```

**绑定HTML Class：**
```html
<!-- 对象语法 -->
<div :class="{ active: isActive, 'text-danger': hasError }"></div>

<!-- 数组语法 -->
<div :class="[activeClass, errorClass]"></div>

<!-- 数组与对象混用 -->
<div :class="[{ active: isActive }, errorClass]"></div>
```

**绑定内联样式：**
```html
<!-- 对象语法 -->
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

<!-- 数组语法 -->
<div :style="[baseStyles, overridingStyles]"></div>
```

### 4. 表单输入绑定进阶

#### 复选框组
```html
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
```

#### 选择框选项
```html
<select v-model="selected">
  <option v-for="option in options" :value="option.value" :key="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```

## 💻 实践代码示例

### 1. 基础数据绑定示例

```html
<!DOCTYPE html>
<html>
<head>
    <title>数据绑定示例</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>
    <div id="app">
        <!-- 文本插值 -->
        <h2>{{ title }}</h2>
        
        <!-- 双向绑定 -->
        <input v-model="message" placeholder="输入消息">
        <p>你输入的是：{{ message }}</p>
        
        <!-- 属性绑定 -->
        <img :src="imageUrl" :alt="imageAlt" style="width: 200px;">
        
        <!-- HTML绑定 -->
        <div v-html="htmlContent"></div>
    </div>

    <script>
        const { createApp } = Vue;
        
        createApp({
            data() {
                return {
                    title: '数据绑定示例',
                    message: 'Hello Vue!',
                    imageUrl: 'https://vuejs.org/images/logo.png',
                    imageAlt: 'Vue.js Logo',
                    htmlContent: '<strong>这是粗体文本</strong>'
                }
            }
        }).mount('#app');
    </script>
</body>
</html>
```

### 2. 条件渲染示例

```html
<div id="conditional-app">
    <h2>条件渲染示例</h2>
    
    <!-- v-if/v-else -->
    <div>
        <button @click="toggleLogin">{{ isLoggedIn ? '退出' : '登录' }}</button>
        <div v-if="isLoggedIn">
            <h3>欢迎回来！</h3>
            <p>用户权限：{{ userRole }}</p>
        </div>
        <div v-else>
            <h3>请先登录</h3>
            <input v-model="username" placeholder="用户名">
            <input v-model="password" type="password" placeholder="密码">
        </div>
    </div>
    
    <!-- v-show -->
    <div>
        <button @click="showDetails = !showDetails">
            {{ showDetails ? '隐藏' : '显示' }}详情
        </button>
        <div v-show="showDetails">
            <h4>详细信息</h4>
            <p>这里是详细内容...</p>
        </div>
    </div>
</div>
```

## 🎯 实践项目：Todo List应用

本模块的实践项目是创建一个功能完整的Todo List应用，包含以下功能：

### 核心功能
1. **添加任务**：输入新任务并添加到列表
2. **删除任务**：删除不需要的任务
3. **标记完成**：标记任务为已完成/未完成
4. **编辑任务**：双击任务可以编辑内容
5. **过滤显示**：显示全部/已完成/未完成任务
6. **批量操作**：全选/全不选、清除已完成

### 技术要点
- 使用`v-model`进行表单数据绑定
- 使用`v-for`渲染任务列表
- 使用`v-if`/`v-show`控制元素显示
- 使用`v-on`处理用户交互
- 使用`v-bind`动态绑定样式和属性

## 📝 作业任务

1. **理论学习**：仔细阅读本模块的理论知识
2. **代码练习**：运行提供的示例代码，理解每个指令的用法
3. **实践项目**：完成Todo List应用的开发
4. **扩展功能**：为Todo List添加以下功能：
   - 任务优先级设置
   - 任务分类
   - 任务搜索
   - 数据持久化（localStorage）

## 🤔 思考题

1. **v-if和v-show的区别是什么？什么时候使用哪个？**
2. **为什么在v-for中需要使用key？key应该如何选择？**
3. **v-model的工作原理是什么？它是如何实现双向绑定的？**
4. **事件修饰符有什么作用？常用的修饰符有哪些？**
5. **如何在Vue中处理表单验证？**

## 🔍 知识点检查

完成本模块后，你应该能够：
- [ ] 熟练使用各种数据绑定语法
- [ ] 理解并应用v-model双向绑定
- [ ] 掌握条件渲染的使用场景
- [ ] 熟练使用v-for进行列表渲染
- [ ] 处理各种用户交互事件
- [ ] 动态绑定HTML属性和样式
- [ ] 完成Todo List项目开发

## 📖 扩展阅读

- [Vue.js官方文档 - 模板语法](https://vuejs.org/guide/essentials/template-syntax.html)
- [Vue.js官方文档 - 条件渲染](https://vuejs.org/guide/essentials/conditional.html)
- [Vue.js官方文档 - 列表渲染](https://vuejs.org/guide/essentials/list.html)
- [Vue.js官方文档 - 事件处理](https://vuejs.org/guide/essentials/event-handling.html)
- [Vue.js官方文档 - 表单输入绑定](https://vuejs.org/guide/essentials/forms.html)

---

**准备好深入学习Vue的数据绑定和指令系统了吗？让我们开始构建你的第一个Todo List应用！**