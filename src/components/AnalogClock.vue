<!-- 
https://basicuse.net/articles/pl/textile/html_css/creating_analog_clock_with_responsive_design_using_animation_in_css3 
https://codepen.io/peterhanus/post/responsive-analog-clock-demo
https://www.telerik.com/blogs/passing-variables-to-css-on-a-vue-component //ToTry
-->
<template>
<div class="clock-case">
  <div class="clock-dial">
    <div 
      v-show="nameBrand" 
      class="clock-brand"
      :style="{ 
        'font-size': clockWidth/12 + 'px',
        'text-shadow': clockWidth/500 + 'px ' + clockWidth/500 + 'px ' + clockWidth/250 + 'px #fff'
      }"
    >{{ nameBrand }}</div>
    <div class="twelve" :style="{ 'font-size': clockWidth/12 + 'px', 'top': clockWidth/40 + 'px' }">12</div>
    <div class="three" :style="{ 'font-size': clockWidth/12 + 'px', 'right': clockWidth/20 + 'px' }">3</div>
    <div class="six" :style="{ 'font-size': clockWidth/12 + 'px', 'bottom': clockWidth/40 + 'px' }">6</div>
    <div class="nine" :style="{ 'font-size': clockWidth/12 + 'px', 'left': clockWidth/20 + 'px' }">9</div>
    <div
      class="clock-dial-line"
      v-for="(n, i) in 60"
      :key="i + '-small'"
      :style="{ 
        'transform-origin': '0 ' + clockWidth/2 + 'px',
        'transform': 'rotate(' + 6*n + 'deg)',
      }"
    ></div>
    <div class="clock-hour-hand" :style="{ 'transform': 'rotate(' + angleHours + 'deg)' }"></div>
    <div class="clock-minute-hand" :style="{ 'transform': 'rotate(' + angleMinutes + 'deg)' }"></div>
    <div class="clock-second-hand" :style="{ 'transform': 'rotate(' + angleSeconds + 'deg)' }"></div>
    <div class="clock-nut"></div>
    <div class="clock-ratio" ref="ratio"></div>
  </div>
</div>
</template>

<script>
let interval = null;

export default {
  props: {
    newDate: Date,
    nameBrand: {
      type: String,
      default: "IPRAGMA"
    }
  },

  data() {
    return {
      currentDate: new Date(),
      clockWidth: null,
    };
  },

  mounted() {
    this.handleResize()
    // https://github.com/vuejs/vue/issues/1915
    window.addEventListener('resize', this.handleResize)
  },

  beforeDestroy() {
    // https://github.com/vuejs/vue/issues/1915
    window.removeEventListener('resize', this.handleResize)
  },

  destroyed() {
    clearInterval(interval);
  },

  computed: {
    angleSeconds() {
      return this.currentDate.getSeconds() * 6;
    },
    angleMinutes() {
      return this.currentDate.getMinutes() * 6 + this.currentDate.getSeconds() * (360 / 3600);
    },
    angleHours() {
      return this.currentDate.getHours() * 30 + this.currentDate.getMinutes() * (360 / 720);
    },
  },

  methods: {
    handleResize() {
      // console.log(this.$refs.ratio.clientWidth) 
      if (this.$refs.ratio.clientWidth % 2 == 0) {
        this.clockWidth = this.$refs.ratio.clientWidth 
      } else {
        this.clockWidth = this.$refs.ratio.clientWidth - 1
      }
    },
  },

  watch: {
    newDate: {
      immediate: true,
      handler() {
        if (this.newDate) {
          this.currentDate = this.newDate;
        } else {
          interval = setInterval(() => {
            this.currentDate = new Date();
          }, 1000);
        }
      }
    },
  }
}
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Lato");
@import url("https://fonts.googleapis.com/css?family=Fjalla+One");

.clock-case {
  background-color: #62696F;
  padding: 8%;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
  border-radius: 50%;
}

.clock-dial {
  background-color: #CECED0;
  position: relative;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
  border-radius: 50%;
}

.clock-ratio {
  padding-bottom: 100%;
  height: 0;
  overflow: hidden;
}

.clock-dial-line {
  content: "";
  width: 1%;
  height: 1.5%;
  background: #b3b3b0;
  position: absolute;
  top: 0;
  left: 50%;
  transform: rotate(0deg);
}

.clock-dial-line:nth-of-type(5n) {
  height: 3%;
}

.clock-second-hand {
  width: 1%;
  height: 100%;
  margin-left: -.5%;
  position: absolute;
  left: 50%;
}

.clock-second-hand:before {
  content: "";
  background-color: red;
  width: 100%;
  height: 55%;
  top: 4%;
  position: absolute;
}

.clock-minute-hand {
  width: 2%;
  height: 100%;
  margin-left: -1%;
  position: absolute;
  left: 50%;
}

.clock-minute-hand:before {
  content: "";
  background-color: #404040;
  width: 100%;
  height: 50%;
  top: 8%;
  position: absolute;
}

.clock-hour-hand {
  width: 3%;
  height: 100%;
  margin-left: -1.5%;
  position: absolute;
  left: 50%;
}

.clock-hour-hand:before {
  content: "";
  background-color: #000000;
  width: 100%;
  height: 46%;
  top: 12%;
  position: absolute;
}

.clock-nut {
  position: absolute;
  top: 46%;
  left: 46%;
  width: 8%;
  height: 8%;
  background-color: #000000;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
  border-radius: 50%;
}

.clock-brand {
  position: absolute;
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
  margin: 0 auto;
  width: 50%;
  text-align: center;
  color: #62696F;
}

.twelve,
.three,
.six,
.nine {
  position: absolute;
  font-family: Lato, sans-serif;
  color: #b3b3b0;
}

.twelve {
  left: 50%;
  transform: translateX(-50%);
}

/* http://zerosixthree.se/vertical-align-anything-with-just-3-lines-of-css/ */
.three {
  top: 50%;
  transform: translateY(-50%);
}

.six {
  left: 50%;
  transform: translateX(-50%);
}

.nine {
  top: 50%;
  transform: translateY(-50%);
}
</style>