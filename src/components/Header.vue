<template lang="">
  <div>
    Header {{ total }}
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    total: { type: Number, required: true },
    currentIndex: { type: Number, required: true }
  }
})
</script>

<style lang="">
  
</style>