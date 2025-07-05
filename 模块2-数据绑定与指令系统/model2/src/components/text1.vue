<template>
  <div class="container">
    <div class="form">
      <h4>注册表单</h4>

      <div>
        <label>用户名：</label>
        <input v-model="user.username" placeholder="输入用户名" />
      </div>

      <div>
        <label>密码：</label>
        <input v-model="user.password" type="password" placeholder="输入密码" />
      </div>

      <div>
        <label>邮箱：</label>
        <input v-model="user.email" type="email" placeholder="输入邮箱" />
      </div>

      <div>
        <label>年龄输入：</label>
        <input v-model.number="user.age" type="number" placeholder="输入年龄" min="0" />
      </div>

      <div>
        <label>性别：</label>
        <div>
          <div>
            <input type="radio" id="male" value="male" v-model="user.gender" />
            <label for="male">男</label>
          </div>
          <div>
            <input type="radio" id="female" value="female" v-model="user.gender" />
            <label for="female">女</label>
          </div>
        </div>
      </div>

      <div>
        <label>兴趣爱好：</label>
        <div>
          <div>
            <input type="checkbox" id="reading" value="reading" v-model="user.hobbies" />
            <label for="reading">阅读</label>
          </div>
          <div>
            <input type="checkbox" id="music" value="music" v-model="user.hobbies" />
            <label for="music">音乐</label>
          </div>
          <div>
            <input type="checkbox" id="sports" value="sports" v-model="user.hobbies" />
            <label for="sports">运动</label>
          </div>
          <div>
            <input type="checkbox" id="travel" value="travel" v-model="user.hobbies" />
            <label for="travel">旅游</label>
          </div>
        </div>
      </div>

      <div>
        <label>所在城市：</label>
        <select v-model="user.city">
          <option value="">请选择城市</option>
          <option value="beijing">北京</option>
          <option value="shanghai">上海</option>
          <option value="guangzhou">广州</option>
          <option value="shenzhen">深圳</option>
          <option value="hangzhou">杭州</option>
        </select>
      </div>

      <div>
        <label>个人介绍：</label>
        <textarea v-model="user.bio" placeholder="输入个人介绍"></textarea>
      </div>

      <div>
        <input type="checkbox" id="agree" v-model="user.agreeTerms" />
        <label for="agree">我同意条款</label>
      </div>

      <button @click="submitForm" :disabled="!isFormValid" class="btn" :class="{ success: isFormValid }">
        {{ isFormValid ? '提交注册' : '请完善信息' }}
      </button>
    </div>

    <div class="card">
      <h4>实时预览</h4>
      <div>
        <p><strong>用户名：</strong>{{ user.username || '未填写' }}</p>
        <p><strong>密码：</strong>{{ user.password ? '●'.repeat(user.password.length) : '未填写' }}</p>
        <p><strong>邮箱：</strong>{{ user.email || '未填写' }}</p>
        <p><strong>年龄：</strong>{{ user.age !== null ? user.age : '未填写' }}</p>
        <p><strong>性别：</strong>{{ getGenderText(user.gender) }}</p>
        <p><strong>兴趣爱好：</strong>{{ user.hobbies.length > 0 ? user.hobbies.join(', ') : '未选择' }}</p>
        <p><strong>所在城市：</strong>{{ getCityText(user.city) }}</p>
        <p><strong>自我介绍：</strong>{{ user.bio || '未填写' }}</p>
        <p><strong>同意条款：</strong><span :class="user.agreeTerms ? 'success' : 'error'">{{ user.agreeTerms ? '已同意' : '未同意' }}</span></p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const user = ref({
  username: '',
  password: '',
  email: '',
  age: null,
  gender: '',
  hobbies: [],
  city: '',
  bio: '',
  agreeTerms: false
});

const isFormValid = computed(() => {
  const { username, password, email, age, gender, city, agreeTerms } = user.value;
  return username && password && email && age !== null && gender && city && agreeTerms;
});

function getGenderText(gender) {
  const map = { male: '男', female: '女' };
  return map[gender] || '未选择';
}

function getCityText(city) {
  const map = {
    beijing: '北京',
    shanghai: '上海',
    guangzhou: '广州',
    shenzhen: '深圳',
    hangzhou: '杭州'
  };
  return map[city] || '未选择';
}

function submitForm() {
  if (isFormValid.value) {
    alert('注册成功！');
    // 可选：清空表单
    user.value = {
      username: '',
      password: '',
      email: '',
      age: null,
      gender: '',
      hobbies: [],
      city: '',
      bio: '',
      agreeTerms: false
    };
  } else {
    alert('请完善信息！');
  }
}
</script>

<style scoped>
.container {
  display: flex;
  gap: 20px;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.form, .card {
  flex: 1;
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.form div {
  margin-bottom: 15px;
}

input[type="text"],
input[type="email"],
input[type="password"],
input[type="number"],
textarea,
select {
  width: 100%;
  padding: 6px 10px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

textarea {
  resize: vertical;
  height: 80px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: rgb(255, 255, 255);
  background-color: gray;
  font-size: 14px;
  min-height: 36px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s ease;
}

.btn.success {
  background-color: blue;
}

.card p {
  margin: 8px 0;
}

.success {
  color: green;
}

.error {
  color: red;
}
</style>