<template>
  <div class="users__wrapper">
    <Button class="users__btn" btnName="Добавить" @btnClick="showModal = true"/>
    <UsersModal v-if="showModal" @close="showModal = false" @save="save" :chiefs="chiefs"/>
    <UsersTable v-if="flatUsers.length" :users="flatUsers" @sort="sort"/>
    <UsersEmptyTable v-else/>
  </div>
</template>

<script>
import UsersTable from './UsersTable';
import UsersEmptyTable from './UsersEmptyTable';
import Button from '../../common/Button';
import UsersModal from './UsersModal';

export default {
    name: 'Users',

    components: { 
        UsersTable, 
        UsersEmptyTable, 
        Button, 
        UsersModal 
    },

    data() {
        return {
            users: [],  // graph model (for calculating)
            flatUsers: [],  // flat graph model (for rendering)
            chiefs: [],
            showModal: false
        };
    },

    methods: {
        sort(field) {
            if (field === 'id') {
                this.initFlatUsers();
            } else {
                this.lexicographicSort(field);
            }
        },

        lexicographicSort(field) {
            this.flatUsers.sort((u1, u2) => {
                const a = u1[field], b = u2[field];
                return (a < b) ? -1 : (a > b) ? 1 : 0;
            });
        },

        save(params) {
            this.showModal = false;
            const user = this.addUser(params)
            this.chiefs.push(this.createChief(user));
            this.initFlatUsers();
        },

        // methods for 'users'

        initUsers() {
            this.users = JSON.parse(window.localStorage.getItem('users')) || [];
        },

        addUser({ name, tel, chiefId }) {
            let userId = null;
            let chief = ''

            if (chiefId) {
                chief = this.chiefs.find(chief => chief.id === chiefId);
                userId = chief.id + '.' + (chief.user.children.length + 1);
            } else {
                userId = this.users.length + 1;
            }

            const user = {
                id: userId,
                name,
                tel,
                chiefName: chief.name || '',
                children: []
            };

            if (chiefId) {
                chief.user.children.push(user);
            } else {
                this.users.push(user);
            }

            return user;
        },

        subscribeUsersStore() {
            window.onbeforeunload = () => {
                window.localStorage.setItem('users', JSON.stringify(this.users));
            };
        },

        // methods for 'flat users'

        initFlatUsers() {
            this.flatUsers = [];
            const dfs = (users) => {
                users.forEach(user => {
                    this.flatUsers.push(user);

                    if (user.children) {
                        dfs(user.children);
                    }
                })
            };
            dfs(this.users);
        },

        // methods for 'chiefs'

        initChiefs() {
            this.chiefs = this.flatUsers.map(user => this.createChief(user));
        },

        createChief(user) {
            return {
                id: user.id,
                name: user.name,
                text: `${user.name} (ID: ${user.id})`,
                user
            };
        }
    },

    mounted() {
        this.$nextTick(function () {  
            this.initUsers();
            this.initFlatUsers();
            this.initChiefs();
            this.subscribeUsersStore();
        });
    }
}
</script>

<style scoped>
    .users__wrapper {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        max-width: max-content;
        padding-top: 10px;
    }

    .users__btn {
        margin-bottom: 10px;
    }
</style>
