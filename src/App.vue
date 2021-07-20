<template>
  <div id="app">
    <Header :total="questions.length" :currentIndex="index" :numOfcorrect="numOfCorrect" />
    <b-container fluid class="p-0 h-100 position-fixed">
      <b-container class="bv-example-row">
        <b-row>
          <b-col sm="6" offset="3">
            <Question
              v-if="questions.length"
              :question="questions[index]"
              :increment="increment"
              :answer="answer" />
          </b-col>
        </b-row>
      </b-container>
    </b-container>
  </div>
</template>

<script lang="ts">
import Header from './components/Header.vue';
import Question from './components/Question.vue';
import Vue from 'vue';
import { QuestionModel } from './typings/question.model';

export default Vue.extend({
  name: 'App',
  components: {
    Header,
    Question
  },
  data() {
    return {
      questions: [] as QuestionModel[],
      index: 0,
      numOfCorrect: 0,
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple', { method: 'get' })
      .then(res => res.json())
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  },
  methods: {
    answer(isCorrect: boolean){
      if(isCorrect){
        this.numOfCorrect++;
      }
    },
    increment(){
      if(this.index + 1 == this.questions.length)
        return;

      this.index++;
    }
  }
})

</script>

<style>
html, body {
  height: 100%;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}
</style>
