<template>
    <table>
        <tr>
            <th class="sorted" @click="sort(sortFields.id)">
                <div class="sort-wrapper">
                    ID
                    <div class="sort-tip" :class="{active: isSortField(sortFields.id)}"></div>
                </div>
            </th>
            <th class="sorted" @click="sort(sortFields.name)">
                <div class="sort-wrapper">
                    Имя
                    <div class="sort-tip" :class="{active: isSortField(sortFields.name)}"></div>
                </div>
            </th>
            <th class="sorted" @click="sort(sortFields.tel)">
                <div class="sort-wrapper">
                    Телефон
                    <div class="sort-tip" :class="{active: isSortField(sortFields.tel)}"></div>
                </div>
            </th>
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
    import { sortFields } from './data';

    export default {
        name: 'UsersTable',

        props: ['users'],

        data() {
            return {
                sortFields,
                sortField: sortFields.id
            };
        },

        methods: {
            isSortField(field) {
                return this.sortField === field;
            },

            sort(field) {
                this.sortField = field;
                this.$emit('sort', field);
            }
        }
    }
</script>

<style scoped>
    table {
        border-collapse: collapse;
        border-top: 2px solid #2185D0;
    }

    tr {
        background-color: #fff;
    }
    
    tr:nth-child(odd) {
        background-color: #F9FAFC;
    }

    th, td {
        width: auto;
        padding: 10px 17px;
        border: 1px solid #D8D8D8;
        text-align: left;
    }

    th {
        cursor: no-drop;
    }

    th.sorted {
        transition: .3s ease;
        cursor: pointer;
    }

    .sort-wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 100%;
    }

    .sort-wrapper .sort-tip {
        position: relative;
        height: .6em;
        width: .6em;
    }

    .sort-tip::before,
    .sort-tip::after {
        color: #bcbcbc;
        position: absolute;
        font-size: .6em;
    }

    .sort-tip::before {
        content: '▲';
        top: -5px;
    }

    .sort-tip::after {
        content: '▼';
        bottom: -5px;
    }
    
    .sort-tip.active::before,
    .sort-tip.active::after {
        color: #2185D0;
    }

    th.sorted:hover {
        box-shadow: 0 0 3px 0 #000 inset;
    }
</style>
