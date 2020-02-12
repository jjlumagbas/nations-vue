<template>
  <div id="app">
    <NationsChart :nationsData="nationsData" />
  </div>
</template>

<script>
import NationsChart from './components/NationsChart.vue'
import { json } from "d3-fetch";

const d3 = {
  json
};

export default {
  name: 'app',

  components: {
    NationsChart
  },

  data: function() {
    return {
      nationsData: []
    }
  },

  created: function() {
    d3.json("/data/nations.json").then(data => this.nationsData = data);
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
}
</style>
