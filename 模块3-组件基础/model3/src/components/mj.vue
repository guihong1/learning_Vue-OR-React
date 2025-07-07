<template>
<div>
    <p>技能：{{jn }}</p>
    <p>年龄:{{ age }}</p>
</div>

</template>


<script setup>
import {defineProps} from 'vue'
const props = defineProps({
    jn:{
        type:String,
        require:true
    },
    age:{
        type:Number,
        default:18
        
    }
})

</script>