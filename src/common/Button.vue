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
        background-color: #DDD;
        padding: 7px 25px;
        border: 1px solid #000;
        border-radius: 13px;
        outline: none;
        cursor: pointer;
    }
</style>