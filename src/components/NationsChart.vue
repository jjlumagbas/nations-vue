<template>
  <section>
    <p>{{ year }}</p>
    <YearScrubber v-model="year"/>
  </section>
</template>

<script>
import YearScrubber from './YearScrubber.vue'

export default {
  name: 'NationsChart',

  data() {
    return {
      year: 1800
    }
  },

  components: {
    YearScrubber
  }

}
</script>

<style scoped>
</style>
