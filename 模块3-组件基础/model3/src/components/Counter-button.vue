<template>
    <button @click="handleClick" class="btn btn-primary">
        {{ label }}
    </button>

</template>


<script setup>
import {ref,defineProps,defineEmits} from 'vue'
const props = defineProps({
    label:{
        type:String,
        required:true
    },
    step:{
        type:Number,
        default:1
    }

})
const emit=defineEmits(['increment'])
const handleClick=()=>{
    emit('increment',props.step)
}
</script>