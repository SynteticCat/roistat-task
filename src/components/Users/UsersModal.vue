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
                    <input id="name" type="text" autocomplete="off" 
                        v-model.trim="name" placeholder="Иван">
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
            const params = {
                name: this.name,
                tel: this.tel,
                chiefId: this.chiefId
            };

            if (this.checkFields()) {
                this.$emit('save', params);
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
            const reg = /^[+]7\s\d{3}\s\d{3}\s\d{2}\s\d{2}$/;
            return reg.test(this.tel);
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
        padding: 10px 17px;
        background-color: #fff;
        border-radius: 5px;
    }

    .modal__header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }

    .modal__title {
        font-size: 1.3em;
        font-weight: bold;
    }

    .modal__close {
        cursor: pointer;
        background-color: red;
        margin-top: -15px;
        width: 20px;
        height: 30px;
        border-bottom-left-radius: 5px;
        border-bottom-right-radius: 5px;
        color: #fff;
        display: flex;
        justify-content: center;
        align-items: flex-end;
        padding-bottom: 5px;
        box-sizing: border-box;
    }

    .modal__close:hover {
        
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

    /* input, select {
        border: none;
        outline: none;
        padding: 15px 20px;
        border-radius: 5px;
        border: 1px solid #bbc0c4;
    }

    input:focus {
        border-radius: 5px;
        border: 1px solid #2185D0;
        box-shadow: 0 0 0 4px #6cbbf7;
    } */

    .param__item:nth-child(n + 2) {
        margin-top: 15px;
    }

    .param__item label {
        width: 110px;
        cursor: pointer;
    }

    .param__tip {
        font-size: 12px;
        color: rgb(150, 150, 150);
        text-transform: lowercase;
        font-style: italic;
        text-align: right;
        margin-top: 4px;
    }

    .param__item input,
    .param__item select {
        width: 220px;
        box-sizing: border-box;
        height: 25px;
    }
</style>