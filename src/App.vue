<template>
  <div id="app">
    <Header :total="total" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Question />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script lang="ts">
import Header from './components/Header.vue';
import Question from './components/Question.vue';
import Vue from 'vue';

export default Vue.extend({
  name: 'App',
  components: {
    Header,
    Question
  },
  data() {
    return {
      questions: [],
      index: 0,
      total: 0
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', { method: 'get' })
      .then(res => res.json())
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  }
})

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
