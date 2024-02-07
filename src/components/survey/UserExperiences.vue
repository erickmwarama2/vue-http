<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExpriences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="loading"> Loading...</p>
      <ul v-if="!loading">
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
  data() {
    return {
      results: [],
      loading: false,
    }
  },
  mounted() {
    this.loadExpriences();
  },
  methods: {
    async loadExpriences() {
      this.loading = true;
      try {
        const response = await fetch('https://vue-http-demo-9e3e2-default-rtdb.firebaseio.com/surveys.json');
        if (response.ok) {
          const data = await response.json();
          const results = [];
          for(const id in data) {
            results.push({id, name: data[id].name, rating: data[id].rating});
          }
          // console.log(this.data);
          this.results = results;
          this.loading = false;
        }
      } catch (error) {
        this.results = [];
        this.loading = false;
      }
    }
  },
  components: {
    SurveyResult,
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