<template>
<div class="exercise">
    <div>
        <input v-model="newStudent.name" class="input-field" placeholder="学生姓名">
        <input v-mode.number="newStudent.age" type="number" class="input-field" placeholder="年龄">
        <select v-model="newStudent.grade" class="select-field">
            <option value="">选择年级</option>
            <option value="一年级">一年级</option>
            <option value="二年级">二年级</option>
            <option value="三年级">三年级</option>
        </select>
        <button @click="addStudent" class="btn">添加学生</button>
    </div>

    <div>
        <!-- 学生列表 -->
        <div>
            <h4>学生列表:{{ students.length }}人</h4>
            <div v-if="students.length===0">
                <p>暂无学生数据</p>
            </div>
            <div v-else>
                <div 
                    v-for="(student,index) in students"
                    :key="student.id"
                    >
                    <div>
                        <strong>{{ student.name }}</strong>
                        <span >ID:{{ student.id }}</span>
                        <br>
                        <small>{{ student.age }}岁 -{{ student.grade }}</small>
                    </div>
                    <div>
                        <button @click="removeStudent(index)">删除</button>
                    </div>
                </div>
            </div>
        </div>
    

        <!-- 按年级分组 -->
        <div>
            <h4>按年级分组</h4>
            <div v-for="(gradeStudents,grade) in studentsByGrade" :key="grade">
            <h5>{{ grade }} ({{ gradeStudents.length }}人)</h5>
            <div v-for="student in gradeStudents" :key="student.id">

            {{ student.name }} - {{ student.age }}岁

            </div>
            </div>
        </div>
    </div>

    <!-- 统计信息 -->
   <div class="card">
                <h4>📊 统计信息</h4>
                <div class="grid">
                    <div v-for="(value, key) in statistics" :key="key" class="card">
                        <h5>{{ getStatLabel(key) }}</h5>
                        <p class="highlight">{{ value }}</p>
                    </div>
                </div>
    </div>

     <div class="card">
                <h4>🏫 班级课程表</h4>
                <div v-for="classInfo in classes" :key="classInfo.name" class="demo-area">
                    <h5>{{ classInfo.name }}</h5>
                    <div class="grid">
                        <div v-for="(subjects, day) in classInfo.schedule" :key="day" class="card">
                            <h6>{{ day }}</h6>
                            <ul>
                                <li v-for="(subject, period) in subjects" :key="period">
                                    第{{ period + 1 }}节：{{ subject }}
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
    </div>

    
</div>
</template>


<script setup>
import {ref,computed} from 'vue'
const students =ref([
    { id: 1, name: '张三', age: 8, grade: '一年级' },
    { id: 2, name: '李四', age: 9, grade: '二年级' },
    { id: 3, name: '王五', age: 10, grade: '三年级' }
])
const newStudent =ref({
   
    name:'',
    age:'',
    grade:''
})
const nextId=ref(4);
const classes = ref([
    {
        name:'一班',
        schedule:{
            '周一':['数学','英语'],
            '周二':['物理','化学'],
            '周三':['生物','政治']
        }
    },
    {
        name:'二班',
        schedule:{
            '周一':['历史','地理'],
            '周二':['音乐','美术'],
            '周三':['计算机','信息技术']
        }
    }
])

const studentsByGrade=computed(()=>{
    const groups={};
    students.value.forEach(student =>{
        if(!groups[student.grade])
    {
        groups[student.grade]=[];
    }
    groups[student.grade].push(student);
    })
    return groups;
})

const statistics=computed(()=>{
    return {
        total: students.length,
        averageAge:students.length >0 ? (students.reduce((sum,s)=>sum+s.age,0)/students.length).toFixed(1) :0,
        grades:Object.keys(studentsByGrade).length
    }
})

function addStudent()
{
    if(newStudent.value.name && newStudent.value.age && newStudent.value.grade)
    {
        students.value.push({
            id:nextId.value++,
            name:newStudent.value.name,
            age:newStudent.value.age,
            grade:newStudent.value.grade
        })
    }
    newStudent.value={
        name:'',
        age:'',
        grade:''
    }

}

function removeStudent(index)
{
    students.value.splice(index,1);
}

function getStatLabel(key)
{
    const labels={
        total:'总人数',
        averageAge:'平均年龄',
        grades:'年级数量'
    }
    return labels[key] || key;
}


</script>