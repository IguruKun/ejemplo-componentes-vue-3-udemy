<script setup>
defineProps(['inicio', 'fin', 'maxLength']);
const emit = defineEmits(['prev', 'next'])

</script>
<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button 
            type="button" 
            @click="emit('prev')" 
            class="btn btn-outline-primary"
            :disabled="inicio <= 0"
        >
            Previus
        </button>
        <button 
            type="button" 
            @click="emit('next')" 
            class="btn btn-outline-primary"
            :disabled="fin  >= maxLength"
        >
            Next
        </button>
    </div>
</template>