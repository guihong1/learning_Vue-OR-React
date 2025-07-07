<template>
<div>
    <h4>
        {{ name }}
    </h4>
    <p>
        {{ message }}
    </p>
</div>


</template>


<script setup>
import {defineProps} from 'vue'
const props=defineProps({
    name:{
        type:String,
        required:true
    },
    message:{
        type:String,
        default:'Hello'
    }
})

</script>