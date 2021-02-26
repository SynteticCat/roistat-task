<template>
    <table>
        <tr><th>ID</th><th>Имя</th><th>Телефон</th><th>Начальник</th></tr>
        <tr><td colspan="4">&#127923; Хоть шаром покати...</td></tr>
    </table>
</template>

<script>
    export default {
        name: 'UsersEmptyTable'
    }
</script>

<style scoped>
    table {
        border-collapse: collapse;
    }

    th, td {
        width: auto;
        padding: 10px 17px;
        border: 1px solid #000;
        text-align: left;
    }
</style>
