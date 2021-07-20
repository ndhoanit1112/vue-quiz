<template lang="">
  <div class="pt-5 bg-pink">
    <b-nav tabs>
      <b-nav-item disabled><b>Vue Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{ currentIndex + 1 }}/{{ total }}</b-nav-item>
      <b-nav-item disabled>Correct: {{ numOfcorrect }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    total: Number,
    currentIndex: Number,
    numOfcorrect: {
      type: Number,
      default: 0
    }
  }
})
</script>

<style>
.bg-pink {
  background-color: #FFC0CB;
}
</style>