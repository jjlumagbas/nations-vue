<template>
  <div id="app">
    <NationsChart />
  </div>
</template>

<script>
import NationsChart from './components/NationsChart.vue'

export default {
  name: 'app',
  components: {
    NationsChart
  }
}
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
