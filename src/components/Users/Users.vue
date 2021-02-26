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
    components: { UsersTable, UsersEmptyTable, Button, UsersModal },
    data() {
        return {
            users: [],  // graph model (for calculating)
            flatUsers: [],  // flat graph model (for rendering)
            chiefs: [],
            showModal: false
        };
    },
    methods: {
        sort(by) {
            if (by === 'id') {
                this.initFlatUsers();
            } else if (by === 'name') {
                this.initFlatUsersByName();
            } else if (by === 'tel') {
                this.initFlatUsersByTel();
            }
        },

        save(user) {
            this.showModal = false;
            const newUser = this.createNewUser(user)
            this.initFlatUsers();
            this.chiefs.push(this.createNewChief(newUser));
        },

        createNewUser(user) {
            let newUserId = null;
            let chief = ''

            if (user.chiefId) {
                chief = this.chiefs.find(chief => chief.id === user.chiefId);
                newUserId = chief.id + '.' + (chief.user.children.length + 1);
            } else {
                newUserId = this.users.length + 1;
            }

            const newUser = {
                id: newUserId,
                name: user.name,
                tel: user.tel,
                chiefName: chief.name || '',
                children: []
            };

            if (user.chiefId) {
                chief.user.children.push(newUser);
            } else {
                this.users.push(newUser);
            }

            return newUser;
        },

        createNewChief(user) {
            return {
                id: user.id,
                name: user.name,
                text: `${user.name} (ID: ${user.id})`,
                user
            };
        },

        initChiefs() {
            this.chiefs = this.flatUsers.map(user => this.createNewChief(user));
        },

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

        initFlatUsersByName() {
            this.flatUsers.sort((f1, f2) => {
                const a = f1.name, b = f2.name;
                return (a < b) ? -1 : (a > b) ? 1 : 0;
            });
        },

        initFlatUsersByTel() {
            this.flatUsers.sort((f1, f2) => {
                const a = f1.tel, b = f2.tel;
                return (a < b) ? -1 : (a > b) ? 1 : 0;
            });
        }
    },
    mounted() {
        this.$nextTick(function () {  
            this.users = JSON.parse(window.localStorage.getItem('users')) || [];
            this.initFlatUsers();
            this.initChiefs();
            window.onbeforeunload = () => {
                window.localStorage.setItem('users', JSON.stringify(this.users));
            };
        })
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
    }

    .users__btn {
        margin-bottom: 10px;
    }
</style>
