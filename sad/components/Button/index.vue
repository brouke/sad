<template>
    <div class="btn">
        <button
            :class="class"
        >{{ placeholder }}</button>
    </div>
</template>
<script setup>
const props = defineProps({
    placeholder:{
        type: String,
    }, 
    class:{
        type: String,
    },
})
</script>