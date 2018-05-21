<template>
<div id="app"> 

	<b-container class="bv-example-row">
    <b-row>
        <b-col cols="8" class=""> 

		  <div id="wrapper" class="wp" ref="wrapper">
		    <ul>
		      <li class="item" v-for="i in slider.max" :key="i"  :id="'sa-'+i" :ankerid="i">{{i}}</li> 
		    </ul>
		 
		  </div>
	   	</b-col>
        <b-col>
        	
			  <div class="container">
			    <h1>Vertical</h1>
			    <vue-slider   
			    tooltip="always" 
			    :min= "slider.min"
			    :max= "slider.max"
			    :interval="1"  
			    height="320px" 
			    class="star-slider" 
			    width="4" 
			    v-model="slider.value"
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
        </b-col>

        <b-col>
        	<h2>{{slider.value}}</h2>  
	   		<a href="#" @click.prevent.stop="scrollBack">&lt;</a>
	   		<a href="#" @click.prevent.stop="scrollForward">&gt;</a>
	   	</b-col>
	    </b-row>
 

	</b-container> 

</div>
</template>

<script> 
import vueSlider from 'vue-slider-component' 
import VueScrollTo from 'vue-scrollto'
import VueLodash from 'vue-lodash'

const options = { name: '_' } 

import Vue from 'vue'
  
Vue.use(VueScrollTo)
Vue.use(VueLodash, options) // options is optional

export default {
  components: {
    vueSlider
  },
  name: 'app', 
  methods: {
  	setVal(val) { 
  		this.slider.value = val; 
  	},
    scrollBack() { 
    	let val = (this.slider.value > this.slider.min) ? --this.slider.value : this.slider.value;
    	this.setVal(val);  
    	this.scrollTo();  
    },
    scrollForward() {
    	let val = (this.slider.value < this.slider.max)  ? ++this.slider.value : this.slider.value;
    	this.setVal(val);  
    	this.scrollTo();  
    },
    scrollTo() { 

		this.$scrollTo(document.getElementById('sa-'+this.slider.value), 100, this.scrollOptions) 
    },
  },
  data() {  
    return { 
    	slider: {
			min: 1,
			max: 111,
			value: 1
    	},
    	scrollOptions: {
		    container: '#wrapper', 
		    cancelable: false,
		    offset: 0,
		    x: false,
		    y: true
    	},
    	margin: -15
	}
  },
  created() { 
 		this.setVal = this._.debounce(this.setVal, 100) 
 		this.scrollTo= this._.debounce(this.scrollTo, 100) 
  },
  mounted() {    
	$(this.$refs.wrapper).scroll(() => {
		let wrapperOffset = $(this.$refs.wrapper).offset();
		let windowHeight = $(this.$refs.wrapper).height();		

		let first = undefined;
		let margin = this.margin; 

		$(".item").each( function() {
			let offset = $(this).offset(); 
			let height = $(this).height();  

			if(offset.top < (wrapperOffset.top+windowHeight) && offset.top >= (wrapperOffset.top+margin) && offset.top > margin) {   
			 	first = $(this).attr("ankerid")
				return false
			}
		});
 
		this.setVal(first); 

	}); 
  }
}
</script>

<style> 

.first {
	background-color: red;
}

#app {
  margin: 50px;
}
#wrapper {
  display: block; 
  background-color: pink;
  height: 15rem;
  overflow-y: auto;
  width: 100%; 
}  
 

</style>
