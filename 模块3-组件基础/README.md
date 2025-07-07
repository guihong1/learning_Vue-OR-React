# 模块3：Vue3 组件基础

## 🎯 学习目标

通过本模块的学习，你将掌握：
- 理解Vue3组件的核心概念和设计思想
- 掌握Vue3组件的创建和注册方式（Composition API）
- 熟练使用props进行父子组件通信
- 掌握defineEmits自定义事件的使用
- 理解和使用插槽（slot）进行内容分发
- 掌握动态组件的使用场景
- 深入理解Vue3组件生命周期钩子
- 学会使用setup语法糖和Composition API

## 📚 理论知识

### 1. 组件概念

#### 什么是组件？
组件是Vue.js最强大的功能之一。组件可以扩展HTML元素，封装可重用的代码。在较高层面上，组件是自定义元素，Vue.js的编译器为它添加特殊功能。

#### 组件的优势
- **可重用性**：一次编写，多处使用
- **可维护性**：代码结构清晰，易于维护
- **可测试性**：独立的组件便于单元测试
- **团队协作**：不同开发者可以并行开发不同组件

#### 组件树
```
App (根组件)
├── Header
│   ├── Logo
│   └── Navigation
├── Main
│   ├── Sidebar
│   └── Content
│       ├── Article
│       └── Comments
│           └── Comment
└── Footer
```

### 2. 组件创建与注册

#### 全局注册（Vue3）
```javascript
// Vue3 全局注册组件
const { createApp } = Vue;
const app = createApp({});

app.component('my-component', {
  template: '<div>这是一个全局组件</div>'
});
```

#### 局部注册（Vue3）
```javascript
// Vue3 局部注册组件
const MyComponent = {
  template: '<div>这是一个局部组件</div>'
};

const { createApp } = Vue;
createApp({
  components: {
    'my-component': MyComponent
  }
}).mount('#app');
```

#### 单文件组件（Vue3 .vue文件）
```vue
<template>
  <div class="my-component">
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
  </div>
</template>

<script setup>
// Vue3 Composition API with setup syntax
import { ref } from 'vue';

const title = ref('组件标题');
const content = ref('组件内容');
</script>

<style scoped>
.my-component {
  border: 1px solid #ccc;
  padding: 20px;
}
</style>
```

#### Options API 写法（Vue3兼容）
```vue
<template>
  <div class="my-component">
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
  </div>
</template>

<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      title: '组件标题',
      content: '组件内容'
    }
  }
}
</script>
```

### 3. 父子组件通信

#### Props（父传子）

**基础用法（Vue3 Composition API）：**
```javascript
// 子组件
const ChildComponent = {
  props: ['message'],
  template: '<div>{{ message }}</div>'
};

// 或使用 setup 语法
const ChildComponent = {
  props: ['message'],
  setup(props) {
    return {
      // props 可以直接在模板中使用
    }
  },
  template: '<div>{{ message }}</div>'
};

// 父组件使用
<child-component message="Hello from parent"></child-component>
```

**Props验证（Vue3）：**
```javascript
// Options API 写法
const MyComponent = {
  props: {
    // 基础类型检查
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      default: () => ({ message: 'hello' })
    },
    // 自定义验证函数
    propF: {
      validator: (value) => {
        return ['success', 'warning', 'danger'].includes(value)
      }
    }
  }
};

// Composition API with setup 语法
const MyComponent = {
  props: {
    propA: Number,
    propC: {
      type: String,
      required: true
    }
  },
  setup(props) {
    // 在 setup 中使用 props
    console.log(props.propA, props.propC);
    return {}
  }
};
```

#### defineEmits（子传父 - Vue3）

```javascript
// 子组件 - Options API
const ButtonCounter = {
  emits: ['increment'],
  template: `
    <button @click="incrementCounter">
      {{ counter }}
    </button>
  `,
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter() {
      this.counter += 1
      this.$emit('increment', this.counter)
    }
  }
};

// 子组件 - Composition API with setup
const ButtonCounter = {
  emits: ['increment'],
  setup(props, { emit }) {
    const counter = ref(0);
    
    const incrementCounter = () => {
      counter.value += 1;
      emit('increment', counter.value);
    };
    
    return {
      counter,
      incrementCounter
    }
  },
  template: `
    <button @click="incrementCounter">
      {{ counter }}
    </button>
  `
};

// 父组件使用
<button-counter @increment="onIncrement"></button-counter>
```

### 4. 插槽（Slot）

#### 基础插槽（Vue3）
```javascript
// 子组件 - Options API
const AlertBox = {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
};

// 子组件 - Composition API
const AlertBox = {
  setup() {
    return {}
  },
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
};

// 使用
<alert-box>
  Something bad happened.
</alert-box>
```

#### 具名插槽（Vue3）
```vue
<!-- 子组件 -->
<template>
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<!-- 父组件使用（Vue3 支持简写语法） -->
<base-layout>
  <template #header>
    <h1>这里是页面标题</h1>
  </template>

  <p>这里是主要内容</p>
  <p>这里是更多的主要内容</p>

  <template #footer>
    <p>这里是页面底部</p>
  </template>
</base-layout>

<!-- 或使用完整语法 -->
<base-layout>
  <template v-slot:header>
    <h1>这里是页面标题</h1>
  </template>
  <!-- 其他内容 -->
</base-layout>
```

#### 作用域插槽（Vue3）
```javascript
// 子组件
const TodoList = {
  props: ['todos'],
  template: `
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <slot :todo="todo">
          <!-- 默认内容 -->
          {{ todo.text }}
        </slot>
      </li>
    </ul>
  `
};

// 使用（Vue3 支持解构语法）
<todo-list :todos="todos">
  <template #default="{ todo }">
    <i class="fas fa-check" v-if="todo.isComplete"></i>
    {{ todo.text }}
  </template>
</todo-list>

// 或使用完整语法
<todo-list :todos="todos">
  <template v-slot:default="slotProps">
    <i class="fas fa-check" v-if="slotProps.todo.isComplete"></i>
    {{ slotProps.todo.text }}
  </template>
</todo-list>
```

```vue
<!-- 子组件 .vue 文件写法 -->
<template>
  <ul>
    <li v-for="item in items" :key="item.id">
      <slot :item="item" :index="index"></slot>
    </li>
  </ul>
</template>

<!-- 父组件使用 -->
<todo-list>
  <template v-slot:default="slotProps">
    <span>{{ slotProps.item.text }}</span>
  </template>
</todo-list>
```

### 5. 动态组件（Vue3）

动态组件允许你在同一个挂载点动态切换多个组件。

```javascript
// 定义多个组件
const Home = { template: '<div>Home component</div>' };
const Posts = { template: '<div>Posts component</div>' };
const Archive = { template: '<div>Archive component</div>' };

// 在父组件中使用 - Options API
const { createApp } = Vue;
createApp({
  data() {
    return {
      currentView: 'Home'
    }
  },
  components: {
    Home,
    Posts,
    Archive
  },
  template: `
    <div>
      <button @click="currentView = 'Home'">Home</button>
      <button @click="currentView = 'Posts'">Posts</button>
      <button @click="currentView = 'Archive'">Archive</button>
      
      <component :is="currentView"></component>
    </div>
  `
}).mount('#app');

// Composition API 写法
const { createApp, ref } = Vue;
createApp({
  setup() {
    const currentView = ref('Home');
    
    return {
      currentView
    }
  },
  components: {
    Home,
    Posts,
    Archive
  },
  template: `
    <div>
      <button @click="currentView = 'Home'">Home</button>
      <button @click="currentView = 'Posts'">Posts</button>
      <button @click="currentView = 'Archive'">Archive</button>
      
      <component :is="currentView"></component>
    </div>
  `
}).mount('#app');
```

#### 单文件组件写法（Vue3）
```vue
<template>
  <div>
    <button 
      v-for="tab in tabs" 
      :key="tab"
      @click="currentTab = tab"
      :class="{ active: currentTab === tab }"
    >
      {{ tab }}
    </button>
    
    <!-- 动态组件 -->
    <component :is="currentTabComponent"></component>
  </div>
</template>

<script setup>
// Vue3 Composition API with setup syntax
import { ref, computed } from 'vue';
import TabHome from './TabHome.vue';
import TabPosts from './TabPosts.vue';
import TabArchive from './TabArchive.vue';

const currentTab = ref('Home');
const tabs = ref(['Home', 'Posts', 'Archive']);

const currentTabComponent = computed(() => {
  return 'Tab' + currentTab.value;
});
</script>

<!-- Options API 写法（Vue3兼容） -->
<script>
export default {
  components: {
    TabHome,
    TabPosts,
    TabArchive
  },
  data() {
    return {
      currentTab: 'Home',
      tabs: ['Home', 'Posts', 'Archive']
    }
  },
  computed: {
    currentTabComponent() {
      return 'Tab' + this.currentTab
    }
  }
}
</script>
```

#### keep-alive缓存组件（Vue3）
```vue
<template>
  <!-- 使用 keep-alive 缓存动态组件 -->
  <keep-alive>
    <component :is="currentView"></component>
  </keep-alive>

  <!-- 包含/排除特定组件 -->
  <keep-alive include="Home,Posts">
    <component :is="currentView"></component>
  </keep-alive>

  <keep-alive exclude="Archive">
    <component :is="currentView"></component>
  </keep-alive>
</template>

<script setup>
// Vue3 中 keep-alive 的生命周期钩子
import { onActivated, onDeactivated } from 'vue';

// 在被缓存的组件中使用
onActivated(() => {
  console.log('组件被激活');
});

onDeactivated(() => {
  console.log('组件被缓存');
});
</script>
```

### 6. 组件生命周期详解（Vue3）

#### Options API 生命周期钩子（Vue3兼容）
```javascript
export default {
  // 创建阶段
  beforeCreate() {
    console.log('beforeCreate: 实例初始化之后，数据观测和事件配置之前')
  },
  created() {
    console.log('created: 实例创建完成，数据观测、属性和方法的运算、事件回调已配置')
  },
  
  // 挂载阶段
  beforeMount() {
    console.log('beforeMount: 挂载开始之前被调用')
  },
  mounted() {
    console.log('mounted: 实例挂载到DOM上')
  },
  
  // 更新阶段
  beforeUpdate() {
    console.log('beforeUpdate: 数据更新时调用，发生在虚拟DOM重新渲染之前')
  },
  updated() {
    console.log('updated: 数据更改导致的虚拟DOM重新渲染之后')
  },
  
  // 销毁阶段（Vue3中改名）
  beforeUnmount() {
    console.log('beforeUnmount: 实例卸载之前调用')
  },
  unmounted() {
    console.log('unmounted: 实例卸载后调用')
  }
}
```

#### Composition API 生命周期钩子（Vue3）
```javascript
// setup 语法糖写法
<script setup>
import { 
  onBeforeMount, 
  onMounted, 
  onBeforeUpdate, 
  onUpdated, 
  onBeforeUnmount, 
  onUnmounted,
  onActivated,
  onDeactivated
} from 'vue';

// 挂载阶段
onBeforeMount(() => {
  console.log('beforeMount: 挂载开始之前被调用');
});

onMounted(() => {
  console.log('mounted: 实例挂载到DOM上');
});

// 更新阶段
onBeforeUpdate(() => {
  console.log('beforeUpdate: 数据更新时调用');
});

onUpdated(() => {
  console.log('updated: 数据更改导致的虚拟DOM重新渲染之后');
});

// 卸载阶段
onBeforeUnmount(() => {
  console.log('beforeUnmount: 实例卸载之前调用');
});

onUnmounted(() => {
  console.log('unmounted: 实例卸载后调用');
});

// keep-alive 相关
onActivated(() => {
  console.log('activated: 被 keep-alive 缓存的组件激活时调用');
});

onDeactivated(() => {
  console.log('deactivated: 被 keep-alive 缓存的组件停用时调用');
});
</script>

// setup 函数写法
export default {
  setup() {
    onMounted(() => {
      console.log('组件已挂载');
    });
    
    onUnmounted(() => {
      console.log('组件已卸载');
    });
    
    return {}
  }
}
```

#### Vue3 生命周期对比表
| Vue2 | Vue3 Options API | Vue3 Composition API |
|------|------------------|----------------------|
| beforeCreate | beforeCreate | setup() |
| created | created | setup() |
| beforeMount | beforeMount | onBeforeMount |
| mounted | mounted | onMounted |
| beforeUpdate | beforeUpdate | onBeforeUpdate |
| updated | updated | onUpdated |
| beforeDestroy | beforeUnmount | onBeforeUnmount |
| destroyed | unmounted | onUnmounted |
| activated | activated | onActivated |
| deactivated | deactivated | onDeactivated |

#### 生命周期应用场景
- **created**: 进行数据初始化，调用API获取数据
- **mounted**: 操作DOM，初始化第三方库
- **beforeUpdate**: 在数据更新前访问现有的DOM
- **updated**: 在数据更改后操作DOM
- **beforeDestroy**: 清理定时器，解绑事件监听器

## 🚀 实践项目：可复用的卡片组件库

我们将创建一个功能丰富的卡片组件库，包含：
- 基础卡片组件
- 用户信息卡片
- 产品展示卡片
- 统计数据卡片
- 卡片列表容器

### 项目结构
```
模块3-组件基础/
├── 卡片组件库.html
├── components/
│   ├── BaseCard.vue
│   ├── UserCard.vue
│   ├── ProductCard.vue
│   ├── StatCard.vue
│   └── CardList.vue
└── assets/
    └── styles.css
```

## 📝 作业任务

### 基础任务
1. 创建一个简单的按钮组件，支持不同的样式和大小
2. 实现父子组件通信，子组件向父组件传递点击事件
3. 使用插槽创建一个模态框组件

### 进阶任务
1. 创建一个表格组件，支持动态列配置
2. 实现一个标签页组件，使用动态组件切换内容
3. 创建一个表单组件，包含多种输入类型的子组件

### 挑战任务
1. 实现一个树形组件，支持递归渲染
2. 创建一个虚拟滚动列表组件
3. 实现一个拖拽排序的列表组件

## 🤔 思考题

1. 组件的props为什么是单向数据流？这样设计有什么好处？
2. 什么时候使用具名插槽，什么时候使用作用域插槽？
3. 动态组件的keep-alive有什么作用？如何使用？
4. 组件的name属性有什么作用？
5. 如何在组件之间共享样式？scoped样式的原理是什么？

## 📖 扩展阅读

- [Vue.js官方文档 - 组件基础](https://cn.vuejs.org/guide/essentials/component-basics.html)
- [Vue.js官方文档 - 深入了解组件](https://cn.vuejs.org/guide/components/registration.html)
- [Vue.js官方文档 - 插槽](https://cn.vuejs.org/guide/components/slots.html)
- [Vue.js组件设计模式](https://github.com/chrisvfritz/vue-enterprise-boilerplate)
- [Vue.js组件库开发指南](https://github.com/ElementUI/element)

## 🎯 学习检查清单

完成本模块学习后，你应该能够：
- [ ] 理解组件化开发的思想和优势
- [ ] 熟练创建和注册Vue组件
- [ ] 掌握props的使用和验证
- [ ] 熟练使用$emit进行组件通信
- [ ] 理解和使用各种类型的插槽
- [ ] 掌握动态组件的使用场景
- [ ] 深入理解组件生命周期
- [ ] 能够设计和开发可复用的组件
- [ ] 掌握组件间的通信模式
- [ ] 理解组件的最佳实践

---

**准备好深入学习Vue组件了吗？让我们开始实践项目的开发！**