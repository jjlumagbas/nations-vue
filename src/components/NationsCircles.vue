<template>
  <g>
    <circle v-for="d in nations" :key="d.name"
      stroke="white" :cx="scale.x(d.income)" :cy="scale.y(d.lifeExpectancy)"
      :r="scale.radius(d.population)" 
      :fill="scale.color(d.region)">
      <title>{{ circleLabel(d.name, d.region) }}</title>
    </circle> 
  </g>
</template>

<script>

export default {
  name: "NationsCircles",

  props: {
    nations: {
      type: Array,
      required: true
    },
    scale: {
      type: Object,
      required: true
    }
  },

  methods: {
    circleLabel: (name, region) => [name, region].join("\n")
  }
  
};
</script>

<style scoped>
</style>
