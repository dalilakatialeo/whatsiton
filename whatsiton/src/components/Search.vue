<template>
  <q-col>
    <q-row class="flex-center" v-if="searchResults.length === 0">
      <q-col>
        <q-input
          class="custom-input-text-color"
          v-model="inputValue"
          @keyup.enter="submitForm"
        />
      </q-col>
      <q-col class="button-col">
        <q-btn
          v-if="!loading"
          label="Whatsiton?"
          @click="submitForm"
          class="button-hover-active"
          style="
            font-family: 'Dela Gothic One', cursive;
            font-size: 1.1rem;
            text-transform: none;
            margin-top: 40px;
            justify-items: center;
            color: #1c1937;
            background-color: #ffc107;
            border: 1px solid #ffffff;
          "
        >
        </q-btn
        ><q-spinner-dots v-else color="white" size="50px" />
      </q-col>
    </q-row>
  </q-col>
</template>


<script>
import axios from "axios";

export default {
  name: "Search",
  data() {
    return {
      inputValue: "",
      searchResults: [],
      result: null,
      moviePoster: null,
      loading: false,
    };
  },
  created() {
    this.clear();
  },
  methods: {
    async fetchData() {
      this.loading = true;
      this.loadingProgress = 0;
      const apiKey = "006bac0037msh933b904ad778e5ep1df481jsn7af220bf0888";
      const apiUrl = "streaming-availability.p.rapidapi.com";
      const options = {
        method: "GET",
        url: "https://streaming-availability.p.rapidapi.com/search/title",
        params: {
          title: this.inputValue,
          country: "au",
          show_type: "all",
          output_language: "en",
        },
        headers: {
          "X-RapidAPI-Key": apiKey,
          "X-RapidAPI-Host": apiUrl,
        },
      };
      try {
        const response = await axios.request(options);
        this.searchResults = response.data;
        this.result = this.searchResults.result[0];
        console.log(response.data, "RESPONSE FROM API");
        this.error = null;
      } catch (error) {
        this.error = error;
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    async getMoviePoster() {
      const apiKey = "979d5cdb9e06568892694ef696e0189c";
      const apiUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.inputValue}`;
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
      if (this.inputValue === "") {
        alert("You need to enter a title!");
      } else {
        this.loading = true;
        console.log(this.inputValue);
        await this.fetchData(this.inputValue);
        console.log("RESULTS FROM SEARCH", this.searchResults);
        console.log("first item", this.result);
        await this.getMoviePoster(this.inputValue);
        this.$emit("searchResultsUpdated", this.searchResults);
        this.$emit("moviePosterRetrieved", this.moviePoster);
      }
    },
    delay(ms) {
      return new Promise((resolve) => setTimeout(resolve, ms));
    },
    clear() {
      this.searchResults = [];
      this.inputValue = "";
      this.result = null;
      this.moviePoster = null;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Dela+Gothic+One&family=Montserrat:wght@300;700&family=Staatliches&display=swap");

.custom-input-text-color input {
  width: 40vw;
  text-align: center;
  font-size: 1.2rem;
  color: white;
  border: 1px solid white;
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
</style>