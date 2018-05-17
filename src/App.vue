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
      value4: 50
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
#app {
  margin: 50px;
}

</style>
