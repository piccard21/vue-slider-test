<template>
<div id="app"> 

	<b-container class="bv-example-row">
    <b-row>
        <b-col cols="8" class=""> 

		  <div id="wrapper" class="wp" ref="wrapper">
		    <ul>
		      <li class="item" v-for="i in max" :key="i"  :id="'sa-'+i" :ankerid="i">{{i}}</li> 
		    </ul>
		 
		  </div>
	   	</b-col>
        <b-col>
        	
			  <div class="container">
			    <h1>Vertical</h1>
			    <vue-slider   
			    tooltip="always" 
			    :min= "min"
			    :max= "max"
			    :interval="1"  
			    height="320px" 
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
        </b-col>

        <b-col>
        	<h2>{{val}}</h2>  
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
  		this.val = val; 
  	},
    scrollBack() { 
    	let val = (this.val > this.min) ? --this.val : this.val;
    	this.setVal(val);  
    	this.scrollTo();  
    },
    scrollForward() {
    	let val = (this.val < this.max)  ? ++this.val : this.val;
    	this.setVal(val);  
    	this.scrollTo();  
    },
    scrollTo() {    
		let options = {
		    container: '#wrapper', 
		    cancelable: false,
		    offset: 0,
		    x: false,
		    y: true
		}

		VueScrollTo.scrollTo(document.getElementById('sa-'+this.val), 100, options) 
    },
  },
  data() {  
    return { 
		min: 1,
		max: 111,
		val: 1
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
		let margin = -15; 

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
