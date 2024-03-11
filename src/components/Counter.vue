<script setup>

    defineProps({
        count: Number
    })

</script>

<template>
    
    <div class="counter">
        count: {{ count }}
    </div>

</template>

<style scoped>
    .counter {
        padding-bottom: 20px;
    }
</style>