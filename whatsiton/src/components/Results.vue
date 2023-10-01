
<template>
  <q-col>
    <q-row class="results-row">
      <q-card class="results-card" v-if="results.length !== 0 && poster">
        <q-card-title class="results-title">{{
          result ? result.title : ""
        }}</q-card-title>
        <q-card-text v-if="streamingInfo">
          is on
          {{ result ? streamingInfo.toUpperCase() : "" }}
        </q-card-text>
        <q-card-text v-else> is not on any streaming platforms </q-card-text>
      </q-card>
      <div style="text-align: center">
        <img
          v-if="poster"
          class="movie-poster"
          :src="poster"
          alt="Movie Poster"
        />
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
    streamingInfo() {
      return this.result.streamingInfo.au
        ? this.result.streamingInfo.au[0].service
        : null;
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

.results-row {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: center;
  justify-content: center;
  height: 90vh;
}
.results-card {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: center;
  text-align: center;
  padding: 5%;
  margin-bottom: 10%;
  background-color: #0d0b24;
  color: white;
  font-size: 1.2rem;
}

.movie-poster {
  width: auto;
  height: 40vh;
  margin-top: 25px;
  margin: 0 auto;
  border: 1px solid white;
}

.button-center {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10%;
}
</style>