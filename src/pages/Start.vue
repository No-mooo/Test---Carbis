<template>
    <div class="address">
        <form class="address__form" @submit.prevent="upListAddress">
            <input type="text" v-model="value" placeholder="Введите адрес">
            <ul v-show="listAddress.length != 0" class="address__listAddress">
                <li
                    v-for="item in listAddress"
                    :key="item.data.fias_id"
                    @click="() => touchAddress(item.value)"
                >
                    {{ item.value }}
                </li>
            </ul>

            <button type="submit" class="address__submit">Поиск</button>
        </form>
    </div>
</template> 

<script>
export default {
    name: 'Start',

    data() {
        return {
            value: '',
            listAddress: [],
        }
    },

    methods: {
        touchAddress(newValue) {
            this.value = newValue;
            this.listAddress = [];
        }
    },

    computed: {
        upListAddress() {
            let url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/suggest/address";
            let options = {
                method: "POST",
                mode: "cors",
                
                headers: {
                    "Content-Type": "application/json",
                    "Accept": "application/json",
                    "Authorization": "Token " + process.env.VUE_APP_TOKEN
                },
                body: JSON.stringify({ query: this.value })
            }

            fetch(url, options)
                .then(response => response.text())
            .then(result => this.listAddress = JSON.parse(result).suggestions);
        }
    },
}
</script>
<style>
.address {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    column-gap: 10px;
}

.address__form {
    position: relative;
}

.address input {
    border-radius: 10px;
    border: 1px solid #eee;
    transition: .3s border-color;
    padding: 10px;
    background-color: rgb(230, 247, 238);
}

.address__listAddress {
    position: absolute;
    top: 100%;
    border-radius: 10px;
    border: 1px solid #a19e9e;
    border-top: none;
    transition: .3s border-color;
    padding: 10px;
    background-color: rgb(255, 255, 255);
    border-radius: 5px;
    box-shadow: 4px 4px #f3f3f3;
}

.address__listAddress li {
    font-size: 14px;
    cursor: pointer;
    font-size: 14px;
    border-radius: 5px;
    padding: 5px;
}

.address__listAddress li:hover {
    background-color: #d7e7e3;
}

.address__listAddress li:not(:last-child) { 
    margin-bottom: 8px;
}

.address__submit {
    border: 1px solid grey;
    border-radius: 10px;
    padding: 10px;
    margin-left: 5px;
}

@media (max-width: 300px) {
    .address__form {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .address__submit {
        width: 65%;
        margin-top: 6px;
    }
}
</style>