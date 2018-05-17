<template>
<div id="app">
  <!-- Do not do anything -->
  <div class="container" v-show="show">
    <h1>Do not do anything</h1>
    <vue-slider ref="slider1" v-model="value1"></vue-slider>
  </div>
  
  <!-- use refresh -->
  <div class="container" v-show="show">
    <h1>use refresh</h1>
    <vue-slider ref="slider2" v-model="value2"></vue-slider>
  </div>
  

  <!-- use v-if -->
  <div class="container" v-if="show">
    <h1>use v-if</h1>
    <vue-slider ref="slider3" v-model="value3"></vue-slider>
  </div>
  
  <!-- use the show attribute -->
  <div class="container" v-show="show">
    <h1>use the show attribute</h1>
    <vue-slider ref="slider4" v-model="value4" :show="show"></vue-slider>
  </div>
  <button @click="show = !show">Toggle</button>

 
  <div id="wrapper" class="wp" ref="wrapper">
    <ul>
      <li v-for="i in 101" :key="i" v-waypoint="{ active: true, callback: onWaypoint, options: intersectionOptions}" :id="'sa-'+i" :ankerid="i">{{i}}</li> 
    </ul>
 
  </div>

  <div class="container" v-show="show">
    <h1>Vertical</h1>
    <vue-slider   
    tooltip="always"
    :tooltip-style="{background: 'red', width: '112px'}"
    :min= "1"
    :max= "max"
    :interval="10"  
    height="320px"
    :slider-style="{background: 'red', width: '112px'}"
    class="star-slider" 
    width="4" 
    v-model="val"
    :reverse="true"  
    @callback="scrollTo"
    direction="vertical">

      <div class="diy-tooltip" slot="tooltip" slot-scope="{ value }"> 
        <div id="customLabel" >
          <p>
             {{ value }}
          </p>
        </div>
    </div>
    </vue-slider>
  </div>

</div>
</template>

<script> 
import vueSlider from 'vue-slider-component'
import VueWaypoint from 'vue-waypoint'
import Vue from 'vue'
// Waypoint plugin
Vue.use(VueWaypoint)

export default {
  components: {
    vueSlider
  },
  name: 'app', 
  methods: {
    scrollTo() { 
                window.location.href = "#sa-"+this.val;
    },
    onWaypoint (a) { 
              if (a.going === this.$waypointMap.GOING_IN) {
                console.log('waypoint going in!')
                this.val=a.el.getAttribute("ankerid")
              }

              if (a.direction === this.$waypointMap.DIRECTION_TOP) {
                console.log('waypoint going top!')
              }
    }
  },
  data() {  
    return {
      max: 111,
      val: 1,
    intersectionOptions: {
      root: this.$refs.wrapper,
      rootMargin: '0px 0px 0px 0px',
      thresholds: [0]
    },
      show: true,
      value1: 50,
      value2: 50,
      value3: 50,
      value4: 50,
      value9: 22
    }
  },
  mounted() {    
  },
  watch: {
    show (val) {
      if (val) {
        this.$nextTick(() => this.$refs.slider2.refresh());
      }
    }
  } 
}
</script>

<style> 
.wrapper {
  display: block;

}
.wp {
  background-color: pink;
  height: 5rem;
  overflow-y: auto;
  width: 100%;
}
.vue-slider-dot {
  background-color: green !important;
}
#app {
  margin: 50px;
}

#customLabel {
  background: lightgrey;
  padding: 10px;
}

.star-slider .vue-slider .vue-slider-dot {
  background:  "yellow"
}
#pretty-slider .vue-slider-process {
  background-image: -webkit-linear-gradient(left, #f05b72, #3498db);
}
.custom-tooltip {
  text-align: center;
}
.custom-tooltip img {
  display: block;
}
.custom-label {
  position: absolute;
  bottom: 100%;
  left: 0;
  transform: translate(-50%, -12px);
  margin-left: 3px;
}
.custom-label::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translate(-50%, 5px);
  width: 1px;
  height: 5px;
  background-color: #000;
}
.custom-label.active {
  color: #2980b9;
  font-weight: bold;
}
.custom-label.active::after {
  background-color: #2980b9;
  width: 2px;
}


</style>
