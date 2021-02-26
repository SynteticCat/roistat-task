<template>
    <div class="modal__mask">
        <div class="modal__wrapper">
            <div class="modal__header">
                <span class="modal__title">Добавление пользователя</span>
                <span class="modal__close" @click="$emit('close')">&#10005;</span>
            </div>

            <ul class="param__list">
                <li class="param__item">
                    <label for="name">Имя</label>
                    <input id="name" type="text" autocomplete="off" v-model.trim="name"
                        placeholder="Иван">
                </li>

                <li class="param__item">
                    <label for="tel">Телефон</label>
                    <input id="tel" type="tel" autocomplete="off" v-model.trim="tel"
                        placeholder="+7 777 777 77 77">
                </li>

                <div class="param__tip">в формате: +7 XXX XXX XX XX</div>

                <li class="param__item">
                    <label for="chief">Начальник</label>
                    <select id="chief" v-model="chiefId">
                        <option :value="null">Ничего не выбрано</option>
                        <option v-for="chief in chiefs" :key="chief.id" :value="chief.id">
                            {{ chief.text }}
                        </option>
                    </select>
                </li>
            </ul>

            <Button btnName='Сохранить' @btnClick="save"/>
        </div>
    </div>
</template>

<script>
import Button from '../../common/Button';

export default {
    name: 'UsersModal',
    props: ['chiefs'],
    data() {
        return {
            name: null,
            tel: null,
            chiefId: null
        };
    },
    components: {
        Button
    },
    methods: {
        save() {
            const newUser = {
                name: this.name,
                tel: this.tel,
                chiefId: this.chiefId
            };

            if (this.checkFields()) {
                this.$emit('save', newUser);
            }
        },
        checkFields() {
            if (!this.name) {
                alert('Требуется указать имя.');
                return false;
            }

            if (!this.checkTel()) {
                alert('Требуется указать телефон в заданном  формате.');
                return false;
            }

            return true;
        },
        checkTel() {
            const req1 = /[+]7\s\d{3}\s\d{3}\s\d{2}\s\d{2}/;
            const req2  =/.[+]7\s\d{3}\s\d{3}\s\d{2}\s\d{2}/;
            const req3  =/[+]7\s\d{3}\s\d{3}\s\d{2}\s\d{2}./;
            return req1.test(this.tel) && !req2.test(this.tel) && !req3.test(this.tel);
        }
    }
}
</script>

<style scoped>
    .modal__mask {
        position: fixed;
        z-index: 999;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .modal__wrapper {
        width: auto;
        border: 1px solid #000;
        padding: 10px 17px;
        background-color: #fff;
    }

    .modal__header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }

    .modal__close {
        cursor: pointer;
    }

    .param__list {
        margin: 0;
        padding: 0;
        list-style: none;
        margin-bottom: 20px;
    }

    .param__item {
        display: flex;
        align-items: center;
    }

    .param__item:nth-child(n + 2) {
        margin-top: 15px;
    }

    .param__item label {
        width: 110px;
    }

    .param__tip {
        font-size: 12px;
        color: rgb(150, 150, 150);
        text-transform: lowercase;
        font-style: italic;
        text-align: right;
    }

    .param__item input,
    .param__item select {
        width: 220px;
        box-sizing: border-box;
        height: 25px;
    }
</style>