<template>
  <q-page class="flex flex-center">
    <q-col>
      <Search
        ref="searchComponent"
        @searchResultsUpdated="updateSearchResults"
        @moviePosterRetrieved="showPoster"
      />
      <Results
        v-if="showResults"
        :results="searchResults"
        :poster="moviePoster"
        @clearResultsClicked="clearSearchData"
      />
    </q-col>
  </q-page>
</template>

<script>
import Search from "components/Search.vue";
import Results from "components/Results.vue";

export default {
  name: "PageIndex",
  components: { Search, Results },
  data() {
    return {
      searchResults: [],
      moviePoster: null,
    };
  },
  computed: {
    showResults() {
      return this.searchResults.length > 0;
    },
  },
  methods: {
    updateSearchResults(results) {
      this.searchResults = results.result;
    },
    showPoster(poster) {
      this.moviePoster = poster;
    },
    clearSearchData() {
      this.searchResults = [];
      this.moviePoster = null;
      this.$refs.searchComponent.clear();
    },
  },
};
</script>
