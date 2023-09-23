<template>
    <q-col>
        <q-row class="flex-center" v-if="searchResults.length === 0">
            <q-col>
                <q-input class="custom-input" v-model="inputValue" />
            </q-col>
            <q-col class="button-col">
                <q-btn label="Whatsiton?" @click="submitForm" style="margin-top: 40px; justify-items: center;" />
            </q-col>
        </q-row>
        <q-row v-else>
            <q-card>
                <q-card-title>{{ result ? result.title : '' }}</q-card-title> <br />
                <q-card-subtitle></q-card-subtitle>
                <q-card-text>It's on {{ result ? result.streamingInfo.au[0].service : '' }}</q-card-text>
            </q-card>
            <q-btn label="Start over" @click="clear" style="margin-top: 40px; justify-items: center;" />
        </q-row>
    </q-col>
</template>


<script>

import axios from "axios"

export default {
    name: 'Search',
    data() {
        return {
            inputValue: '',
            searchResults: [],
            result: null,
        }
    },
    created() {
        clear(this.searchResults)
    },
    methods: {
        async fetchData() {
            const options = {
                method: 'GET',
                url: 'https://streaming-availability.p.rapidapi.com/search/title',
                params: {
                    title: this.inputValue,
                    country: 'au',
                    show_type: 'all',
                    output_language: 'en'
                },
                headers: {
                    'X-RapidAPI-Key': '00bcc396camshd1a3bd2e2c4bad5p129a25jsnbaece510eedf',
                    'X-RapidAPI-Host': 'streaming-availability.p.rapidapi.com'
                }
            };
            try {
                const response = await axios.request(options);
                this.searchResults = response.data;
                this.result = this.searchResults.result[0]
                console.log(response.data, 'RESPONSE FROM API')
                this.error = null;
            } catch (error) {
                this.error = error;
                console.error(error);
            }
        },
        async submitForm() {
            console.log(this.inputValue)
            await this.fetchData(this.inputValue)
            console.log('RESULTS', this.searchResults)
            console.log('first item', this.result)
        },
        clear() {
            this.searchResults = []
            this.inputValue = ''
            console.log(this.searchResults.length === 0)
        }
    }
}
</script>

<style>
input {
    text-align: center;
}

.button-col {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}
</style>