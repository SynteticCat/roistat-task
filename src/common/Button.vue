<template>
    <button type="button" @click="$emit('btnClick')">
        {{ btnName }}
    </button>
</template>

<script>
    export default {
        name: 'Button',
        props: ['btnName'],
        emits: ['btnClick']
    }
</script>

<style scoped>
    button {
        background-color: #2185D0;
        color: #fff;
        border: none;
        outline: none;
        cursor: pointer;
        padding: 10px 20px;
        border-radius: 5px;
    }
</style>