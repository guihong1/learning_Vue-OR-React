# 模块4：计算属性与侦听器

## 🎯 学习目标
- 深入理解计算属性（computed）的概念和使用场景
- 掌握侦听器（watch）的各种用法
- 理解computed、watch、methods三者的区别和选择
- 学会深度侦听和立即执行的技巧
- 通过购物车项目实践计算属性的实际应用

## 📚 理论知识

### 1. 计算属性（Computed）

#### 基本概念
计算属性是基于响应式依赖进行缓存的。只有在相关响应式依赖发生改变时才会重新求值。

#### Vue 3 语法示例

**Options API：**
```javascript
export default {
  data() {
    return {
      firstName: 'John',
      lastName: 'Doe',
      items: [
        { name: 'Apple', price: 10, quantity: 2 },
        { name: 'Banana', price: 5, quantity: 3 }
      ]
    }
  },
  computed: {
    // 基础计算属性
    fullName() {
      return this.firstName + ' ' + this.lastName
    },
    
    // 复杂计算属性
    totalPrice() {
      return this.items.reduce((total, item) => {
        return total + (item.price * item.quantity)
      }, 0)
    },
    
    // 带getter和setter的计算属性
    fullNameWithSetter: {
      get() {
        return this.firstName + ' ' + this.lastName
      },
      set(newValue) {
        const names = newValue.split(' ')
        this.firstName = names[0]
        this.lastName = names[names.length - 1]
      }
    }
  }
}
```

**Composition API：**
```javascript
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')
const items = ref([
  { name: 'Apple', price: 10, quantity: 2 },
  { name: 'Banana', price: 5, quantity: 3 }
])

// 基础计算属性
const fullName = computed(() => {
  return firstName.value + ' ' + lastName.value
})

// 复杂计算属性
const totalPrice = computed(() => {
  return items.value.reduce((total, item) => {
    return total + (item.price * item.quantity)
  }, 0)
})

// 带getter和setter的计算属性
const fullNameWithSetter = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  set(newValue) {
    const names = newValue.split(' ')
    firstName.value = names[0]
    lastName.value = names[names.length - 1]
  }
})
</script>
```

### 2. 侦听器（Watch）

#### 基本概念
侦听器用于观察和响应Vue实例上的数据变动。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

#### Vue 3 语法示例

**Options API：**
```javascript
export default {
  data() {
    return {
      question: '',
      answer: 'Questions usually contain a question mark. ;-)',
      user: {
        name: 'John',
        age: 25,
        address: {
          city: 'Beijing',
          street: 'Main St'
        }
      },
      items: []
    }
  },
  watch: {
    // 基础侦听
    question(newQuestion, oldQuestion) {
      if (newQuestion.includes('?')) {
        this.getAnswer()
      }
    },
    
    // 深度侦听对象
    user: {
      handler(newUser, oldUser) {
        console.log('User changed:', newUser)
      },
      deep: true
    },
    
    // 立即执行
    'user.name': {
      handler(newName, oldName) {
        console.log('Name changed:', newName)
      },
      immediate: true
    },
    
    // 侦听数组
    items: {
      handler(newItems, oldItems) {
        console.log('Items changed:', newItems.length)
      },
      deep: true
    }
  },
  methods: {
    getAnswer() {
      this.answer = 'Thinking...'
      // 模拟异步操作
      setTimeout(() => {
        this.answer = 'Yes, that is a question!'
      }, 1000)
    }
  }
}
```

**Composition API：**
```javascript
<script setup>
import { ref, watch, watchEffect } from 'vue'

const question = ref('')
const answer = ref('Questions usually contain a question mark. ;-)')
const user = ref({
  name: 'John',
  age: 25,
  address: {
    city: 'Beijing',
    street: 'Main St'
  }
})
const items = ref([])

// 基础侦听
watch(question, (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    getAnswer()
  }
})

// 深度侦听对象
watch(user, (newUser, oldUser) => {
  console.log('User changed:', newUser)
}, { deep: true })

// 立即执行
watch(() => user.value.name, (newName, oldName) => {
  console.log('Name changed:', newName)
}, { immediate: true })

// 侦听多个源
watch([question, () => user.value.name], ([newQuestion, newName], [oldQuestion, oldName]) => {
  console.log('Question or name changed')
})

// watchEffect - 自动追踪依赖
watchEffect(() => {
  console.log(`Question is: ${question.value}`)
  console.log(`User name is: ${user.value.name}`)
})

const getAnswer = () => {
  answer.value = 'Thinking...'
  setTimeout(() => {
    answer.value = 'Yes, that is a question!'
  }, 1000)
}
</script>
```

### 3. Computed vs Watch vs Methods

| 特性 | Computed | Watch | Methods |
|------|----------|-------|----------|
| **缓存** | ✅ 有缓存，依赖不变时不重新计算 | ❌ 无缓存 | ❌ 无缓存 |
| **响应式依赖** | ✅ 自动追踪依赖 | ✅ 手动指定侦听目标 | ❌ 手动调用 |
| **异步操作** | ❌ 不支持 | ✅ 支持 | ✅ 支持 |
| **副作用** | ❌ 应该是纯函数 | ✅ 可以有副作用 | ✅ 可以有副作用 |
| **使用场景** | 数据转换、格式化 | 数据变化时的异步操作 | 事件处理、用户交互 |

#### 选择指南
- **使用 Computed**：当你需要根据现有数据计算出新数据时
- **使用 Watch**：当你需要在数据变化时执行异步操作或复杂逻辑时
- **使用 Methods**：当你需要处理用户交互或执行不依赖于特定数据的操作时

### 4. 高级用法

#### 停止侦听器
```javascript
// Composition API
const stopWatcher = watch(source, callback)

// 停止侦听
stopWatcher()

// 在组件卸载时自动停止
watchEffect((onInvalidate) => {
  const token = performAsyncOperation()
  
  onInvalidate(() => {
    // 清理操作
    token.cancel()
  })
})
```

#### 刷新时机
```javascript
// 在组件更新后触发
watch(source, callback, {
  flush: 'post'
})

// 同步触发
watch(source, callback, {
  flush: 'sync'
})
```

## 🚀 实践项目：购物车计算系统

### 项目功能
1. **商品列表展示**：显示商品名称、价格、库存
2. **购物车管理**：添加/删除商品、修改数量
3. **实时计算**：小计、总计、折扣、税费
4. **优惠券系统**：支持不同类型的优惠券
5. **库存监控**：实时监控库存变化

### 技术要点
- 使用计算属性计算购物车总价
- 使用侦听器监控库存变化
- 实现复杂的价格计算逻辑
- 处理异步的优惠券验证

## 📝 作业任务

### 基础任务
1. **计算属性练习**：创建一个学生成绩管理系统，计算平均分、及格率
2. **侦听器练习**：创建一个搜索框，实现防抖搜索功能
3. **综合练习**：实现一个简单的表单验证系统

### 进阶任务
1. **性能优化**：对比computed和methods在大量数据计算时的性能差异
2. **深度侦听**：实现一个复杂对象的变化追踪系统
3. **异步处理**：使用watch实现数据的自动保存功能

## 🤔 思考题

1. 为什么计算属性比方法更适合用于数据转换？
2. 在什么情况下应该使用深度侦听？深度侦听有什么性能影响？
3. watchEffect和watch的区别是什么？各自的使用场景？
4. 如何在Vue 3中实现一个可复用的防抖hook？
5. 计算属性的setter什么时候会用到？有什么注意事项？

## 📖 扩展阅读

- [Vue 3 官方文档 - 计算属性](https://vuejs.org/guide/essentials/computed.html)
- [Vue 3 官方文档 - 侦听器](https://vuejs.org/guide/essentials/watchers.html)
- [Vue 3 Composition API 深入指南](https://vuejs.org/guide/extras/composition-api-faq.html)
- [响应式系统原理](https://vuejs.org/guide/extras/reactivity-in-depth.html)

## ✅ 学习检查清单

- [ ] 理解计算属性的缓存机制
- [ ] 掌握计算属性的getter和setter用法
- [ ] 理解侦听器的基本用法
- [ ] 掌握深度侦听和立即执行
- [ ] 了解computed、watch、methods的区别
- [ ] 能够选择合适的方式处理不同场景
- [ ] 完成购物车计算系统项目
- [ ] 掌握Vue 3 Composition API的相关用法
- [ ] 理解侦听器的清理和停止机制
- [ ] 能够处理复杂的异步侦听场景

---

**学习建议**：
1. 重点理解计算属性的缓存机制，这是性能优化的关键
2. 多练习侦听器的各种配置选项，特别是深度侦听
3. 通过实际项目体验三种方式的不同使用场景
4. 关注Vue 3 Composition API的新特性和优势