<template>
  <section>
    <svg :viewBox="chartViewbox">
      <NationsCircles :nations="dataThisYear" :scale="scale"/>
    </svg>
    <p>{{ year }}</p>
    <YearScrubber v-model="year"/>
  </section>
</template>

<script>
import YearScrubber from './YearScrubber.vue'
import NationsCircles from './NationsCircles.vue'

import { bisector, descending } from "d3-array";
import { scaleLog, scaleLinear, scaleSqrt, scaleOrdinal } from "d3-scale";
import { schemeTableau10 } from "d3-scale-chromatic";

const d3 = { bisector, descending, scaleLog, scaleLinear, scaleSqrt, scaleOrdinal, schemeTableau10 };

export default {
  name: 'NationsChart',

  components: {
    YearScrubber,
    NationsCircles
  },

  props: {
    nationsData: {
      type: Array,
      required: true
    }
  },

  data: function() {
    return {
      year: 1800,
      width: 800,
      height: 600,
      margin: { top: 20, right: 20, bottom: 35, left: 40 },
      bisectYear: d3.bisector(([year]) => year).left,
      scale: null
    }
  },

  computed: {
    dataThisYear: function() {
      return this.dataAt(this.year).sort((a, b) => d3.descending(a.population, b.population)); 
    },

    chartViewbox: function() {
      return [0, 0, this.width, this.height];
    }
  },

  created: function() {
    this.scale = {
      x: d3.scaleLog([200, 1e5], [this.margin.left, this.width - this.margin.right]),
      y: d3.scaleLinear([14, 86], [this.height - this.margin.bottom, this.margin.top]),
      radius: d3.scaleSqrt([0, 5e8], [0, this.width / 24]),
      color: d3.scaleOrdinal(d3.schemeTableau10)
    }
  },

  methods: {
    dataAt: function(year) {
      return this.nationsData.map(d => ({
        name: d.name,
        region: d.region,
        income: this.valueAt(d.income, year),
        population: this.valueAt(d.population, year),
        lifeExpectancy: this.valueAt(d.lifeExpectancy, year)
      }));
    },

    valueAt: function(values, year) {
      const i = this.bisectYear(values, year, 0, values.length - 1);
      const a = values[i];
      if (i > 0) {
        const b = values[i - 1];
        const t = (year - a[0]) / (b[0] - a[0]);
        return a[1] * (1 - t) + b[1] * t;
      }
      return a[1];
    }
  }

}
</script>

<style scoped>
section {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}
</style>
