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



  <div class="container h-100" v-show="show">
    <h1>Vertical</h1>
    <vue-slider   
    tooltip="always"
    :tooltip-style="{background: 'red', width: '112px'}"
    :min= "10"
    :max= "1000"
    :interval="10"  
    height="320px"
    :slider-style="{background: 'red', width: '112px'}"
    class="star-slider"
    :dotSize="22"
    width="4" 
    v-model="value9"
    :reverse="true" 
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

export default {
  components: {
    vueSlider
  },
  name: 'app', 
  data() {
    return {
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
