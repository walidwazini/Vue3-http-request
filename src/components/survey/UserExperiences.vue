<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading ...</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No storages experiences found.
      </p>
      <ul v-else-if="!isLoading && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      const surveysUrl =
        'https://vue-http-demo-7f5e9-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json';
      // const newUrl =  ''
      fetch(surveysUrl)
        .then((res) => {
          // Check if the request is succesful or not
          if (res.ok) {
            return res.json();
          }
        })
        .then((data) => {
          // console.log(data);
          this.isLoading = false;
          const getResults = [];
          for (const id in data) {
            getResults.push({
              idKey: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = getResults;
          console.log(`We get ${this.results[1]}.`);
        });
    },
  },
  mounted() {
    this.loadExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>