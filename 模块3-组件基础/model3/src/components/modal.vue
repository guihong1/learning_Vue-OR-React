<template>
<div v-if="show" class="modal-overlay" @click="handleOverlayClick">
    <div class="modal-container" @click.stop>
        <div class="modal-header">
           <slot name="header">
            <h3>默认标题</h3>
           </slot>
           <button @click="$emit('close')" class="modal-close">X</button>
        </div>
        <div class="modal-body">
            <slot></slot>
        </div>
        <div class="modal-footer" v-if="$slots.footer">
            <slot name="footer">
            </slot>
        </div>
       
    </div>
</div>

</template>


<script setup>
import {defineProps,defineEmits} from 'vue'
const props = defineProps({
        show:{
            type:Boolean,
            default:false
        }
})
const emit=defineEmits(['close']);
const handleOverlayClick=()=>{
    emit('close');
}




</script>