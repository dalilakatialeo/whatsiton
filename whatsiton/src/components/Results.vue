
<template>
  <q-col>
    <q-row class="flex-center">
      <q-card class="results-card" v-if="results.length !== 0">
        <q-card-title>{{ result ? result.title : "" }}</q-card-title> <br />
        <q-card-subtitle></q-card-subtitle>
        <q-card-text>
          <p>
            is on
            {{ result ? result.streamingInfo.au[0].service.toUpperCase() : "" }}
          </p>
        </q-card-text>
      </q-card>
      <div v-if="poster" style="text-align: center">
        <img class="movie-poster" :src="poster" alt="Movie Poster" />
      </div>
      <div class="button-center">
        <q-btn
          label="Start over"
          @click="clearResults"
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
        />
      </div>
    </q-row>
  </q-col>
</template>


<script>
export default {
  name: "Results",
  props: {
    results: Array,
    poster: String,
  },
  created() {
    console.log("Received results prop:", this.results);
    console.log("RECEIVEDPOSTER", this.poster);
  },
  data() {
    return {};
  },
  computed: {
    result() {
      return this.results.length > 0 ? this.results[0] : null;
    },
  },
  methods: {
    clearResults() {
      this.$emit("clearResultsClicked");
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Dela+Gothic+One&family=Montserrat:wght@300;700&family=Staatliches&display=swap");

.results-card {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: center;
  padding: 5%;
  background-color: #0d0b24;
  color: white;
  font-size: 1.2rem;
}

.movie-poster {
  width: auto;
  height: 50vh;
  margin-top: 20px;
  margin: 0 auto;
  border: 1px solid white;
}
.button-center {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 40px;
}
</style>