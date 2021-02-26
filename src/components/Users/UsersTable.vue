<template>
    <table>
        <tr>
            <th class="sorted" @click="$emit('sort', 'id')">ID</th>
            <th class="sorted" @click="$emit('sort', 'name')">Имя</th>
            <th class="sorted" @click="$emit('sort', 'tel')">Телефон</th>
            <th>Начальник</th>
        </tr>
        <tr v-for="user in users" :key="user.id">
            <td>{{ user.id }}</td>
            <td>{{ user.name }}</td>
            <td>{{ user.tel }}</td>
            <td>{{ user.chiefName}}</td>
        </tr>
    </table>
</template>

<script>
    export default {
        name: 'UsersTable',
        props: ['users']
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

    th {
        cursor: no-drop;
    }

    th.sorted {
        transition: .3s ease;
        cursor: pointer;
    }

    th.sorted:hover {
        box-shadow: 0 0 3px 0 #000 inset;
    }
</style>
