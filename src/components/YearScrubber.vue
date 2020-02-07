<template>
  <section>
    <output>{{ year }}</output>
    <form>
      <button class="button is-medium" type="button" @click="togglePlay" v-if="isPlaying">
        <span class="icon is-medium">
          <i class="fas fa-pause" ></i>
        </span>
      </button>
      <button class="button is-medium" type="button" @click="togglePlay" v-else>
        <span class="icon is-medium">
          <i class="fas fa-play" ></i>
        </span>
      </button>
      <input type="range" id="year" v-model="inputYear" :min="yearRange.min" :max="yearRange.max" step="any" @click="stop">
    </form>
  </section>
</template>

<script>
const fps = 15;

export default {
  name: 'YearScrubber',

  data() {
    return {
      inputYear: 1800,
      yearRange: {
        min: 1800,
        max: 2010
      },
      timer: null,
    };
  },

  computed: {
    year: function() {
      return Math.floor(this.inputYear);
    },
    isPlaying: function() {
      return this.timer == null ? false : true;
    }
  },


  methods: {
    reset: function() {
      if (this.year === this.yearRange.max) {
        this.inputYear = this.yearRange.min;
      }
    },

    tick: function() {
      if (this.inputYear < this.yearRange.max) {
        this.inputYear++;
      } else {
        this.stop();
      }
    },

    stop: function() {
      if (this.timer) {
        clearInterval(this.timer)
        this.timer = null;
      }
    },

    start: function() {
      if (!this.timer) {
        this.reset();
        this.timer = setInterval(this.tick, 1000 / fps);
      }
    },

    togglePlay: function() {
      if (this.timer) {
        this.stop();
      } else {
        this.start();
      }
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

form {
  display: flex;
  align-items: center;
  max-width: 800px;
}

form > * {
  margin-right: 1em;
}

input[type=range] {
  height: 31px;
  -webkit-appearance: none;
  margin: 10px 0;
  width: 100%;
}
input[type=range]:focus {
  outline: none;
}
input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 9px;
  cursor: pointer;
  box-shadow: 0px 0px 0px #000000;
  background: #E3E3E3;
  border-radius: 25px;
  border: 0px solid #8A8A8A;
}
input[type=range]::-webkit-slider-thumb {
  box-shadow: 0px 0px 1px #828282;
  border: 1px solid #8A8A8A;
  height: 24px;
  width: 10px;
  border-radius: 6px;
  background: #DADADA;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -8px;
}
input[type=range]:focus::-webkit-slider-runnable-track {
  background: #E3E3E3;
}
input[type=range]::-moz-range-track {
  width: 100%;
  height: 9px;
  cursor: pointer;
  box-shadow: 0px 0px 0px #000000;
  background: #E3E3E3;
  border-radius: 25px;
  border: 0px solid #8A8A8A;
}
input[type=range]::-moz-range-thumb {
  box-shadow: 0px 0px 1px #828282;
  border: 1px solid #8A8A8A;
  height: 24px;
  width: 10px;
  border-radius: 6px;
  background: #DADADA;
  cursor: pointer;
}
input[type=range]::-ms-track {
  width: 100%;
  height: 9px;
  cursor: pointer;
  background: transparent;
  border-color: transparent;
  color: transparent;
}
input[type=range]::-ms-fill-lower {
  background: #E3E3E3;
  border: 0px solid #8A8A8A;
  border-radius: 50px;
  box-shadow: 0px 0px 0px #000000;
}
input[type=range]::-ms-fill-upper {
  background: #E3E3E3;
  border: 0px solid #8A8A8A;
  border-radius: 50px;
  box-shadow: 0px 0px 0px #000000;
}
input[type=range]::-ms-thumb {
  margin-top: 1px;
  box-shadow: 0px 0px 1px #828282;
  border: 1px solid #8A8A8A;
  height: 24px;
  width: 10px;
  border-radius: 6px;
  background: #DADADA;
  cursor: pointer;
}
input[type=range]:focus::-ms-fill-lower {
  background: #E3E3E3;
}
input[type=range]:focus::-ms-fill-upper {
  background: #E3E3E3;
}
</style>
