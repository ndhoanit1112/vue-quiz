<template>
  <div class="question-box-container mt-5">
    <b-jumbotron>
      <template slot="lead">
        {{ question.question | decodeHTML(fakeTxtArea) }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers" :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer | decodeHTML(fakeTxtArea) }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="success"
        @click.prevent="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button
        variant="secondary"
        @click.prevent="next"
        :disabled="!answered"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script lang="ts">
import { QuestionModel } from '@/typings/question.model'
import Vue, { PropType } from 'vue'
import _ from 'lodash'

export default Vue.extend({
  props: {
    question: Object as PropType<QuestionModel>,
    increment: Function as PropType<() => void>,
    answer: Function as PropType<(isCorrect: boolean) => void>
  },
  data(){
    return {
      fakeTxtArea: document.createElement("textarea"),
      shuffledAnswers: [] as string[],
      selectedIndex: null as number | null,
      correctIndex: null as number | null,
      answered: false,
    }
  },
  watch: {
    question: {
      immediate: true,
      handler(){
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  filters: {
    decodeHTML: (value: string, fakeTxtArea: HTMLTextAreaElement) => {
      fakeTxtArea.innerHTML = value;
      return fakeTxtArea.value;
    }
  },
  methods: {
    shuffleAnswers(){
      let answers = [...this.question.incorrect_answers, this.question.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.question.correct_answer);
    },
    selectAnswer(index: number){
      if(this.answered)
        return;
        
      this.selectedIndex = index;
    },
    submitAnswer(){
      this.answered = true;
      this.answer(this.selectedIndex == this.correctIndex);
    },
    next(){
      this.increment();
    },
    answerClass(index: number){
      let answerClass = '';

      if(this.answered){
        if(this.correctIndex == index){
          answerClass = 'correct';
        }else if(this.selectedIndex == index){
          answerClass = 'incorrect';
        }
      }else{
        if(this.selectedIndex == index){
          answerClass = 'selected';
        }
      }

      return answerClass;
    }
  }
})
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #EEE;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>