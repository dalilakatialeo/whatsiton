<template>
    <q-col>
        <q-row class="flex-center" v-if="searchResults.length === 0">
            <q-col>
                <q-input class="custom-input-text-color" v-model="inputValue" />
            </q-col>
            <q-col class="button-col">
                <q-btn label="Whatsiton?" @click="submitForm" class="button-hover-active"
                    style="font-family: 'Dela Gothic One', cursive; text-transform: none; margin-top: 40px; justify-items: center; color: #1c1937; background-color: #ffc107 ; border: 1px solid #ffffff; " />
            </q-col>
        </q-row>
        <q-row class="flex-center" v-else>
            <q-card class="results-card">
                <q-card-title>{{ result ? result.title : '' }}</q-card-title> <br />
                <q-card-subtitle></q-card-subtitle>
                <q-card-text>
                    <p>is on {{ result ? result.streamingInfo.au[0].service.toUpperCase() : '' }}</p>
                </q-card-text>
            </q-card>
            <div v-if="moviePoster" style="text-align: center">
                <img class="movie-poster" :src="moviePoster" alt="Movie Poster" />
            </div>
            <q-btn label="Start over" @click="clear" class="button-hover-active"
                style="font-family: 'Dela Gothic One', cursive; text-transform: none; margin-top: 40px; justify-items: center; color: black; background-color: #ffc107 ; border: 1px solid #ffffff; " />
        </q-row>
        <q-progress-bar v-if="loading" :value="loadingProgress" color="primary" indeterminate /> </q-col>
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
            moviePoster: null,
            loading: false,
            loadingProgress: 0
        }
    },
    created() {
        this.clear()
    },
    methods: {
        async fetchData() {
            if (this.inputValue === '') {
                alert("You need to enter a title!")
            }
            this.loading = true
            this.loadingProgress = 0
            const apiKey = '006bac0037msh933b904ad778e5ep1df481jsn7af220bf0888'
            const apiUrl = 'streaming-availability.p.rapidapi.com'
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
                    'X-RapidAPI-Key': apiKey,
                    'X-RapidAPI-Host': apiUrl
                }
            };
            try {
                for (let i = 0; i < 100; i++) {
                    await this.delay(50);
                    this.loadingProgress = i;
                }
                const response = await axios.request(options);
                this.searchResults = response.data;
                this.result = this.searchResults.result[0]
                console.log(response.data, 'RESPONSE FROM API')
                this.error = null;
            } catch (error) {
                this.error = error;
                console.error(error);
            }
            finally {
                this.loading = false
                this.loadingProgress = 0
            }
        },
        async getMoviePoster() {
            const apiKey = '979d5cdb9e06568892694ef696e0189c'
            const apiUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.inputValue}`
            try {
                const response = await axios.get(apiUrl);
                const json = response.data;

                if (json.results.length > 0) {
                    const posterPath = json.results[0].poster_path;
                    this.moviePoster = `http://image.tmdb.org/t/p/w500/${posterPath}`;
                } else {
                    this.moviePoster = null;
                }
            } catch (error) {
                console.error(error);
                this.moviePoster = null;
            }
        },

        async submitForm() {
            console.log(this.inputValue)
            await this.fetchData(this.inputValue)
            console.log('RESULTS', this.searchResults)
            console.log('first item', this.result)
            await this.getMoviePoster(this.inputValue)
        },
        delay(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        },
        clear() {
            this.searchResults = []
            this.inputValue = ''
            this.result = null
            this.moviePoster = null
            console.log(this.searchResults.length === 0)
        }
    }
}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Dela+Gothic+One&family=Montserrat:wght@300;700&family=Staatliches&display=swap');

.custom-input-text-color input {
    width: 40vw;
    text-align: center;
    color: white;
    border: 1px solid white
}

.button-col {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}

.button-hover-active:hover,
.button-hover-active:active {
    background-color: #1976d2 !important;
    color: white !important;
}

.results-card {
    padding: 20px;
    background-color: #0d0b24;
    color: white;
}

.movie-poster {
    width: 100px;
    height: auto;
    margin-top: 20px;
    margin: 0 auto;
}
</style>