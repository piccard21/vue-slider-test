<template>
<div id="app"> 

	<b-container class="bv-example-row">
    <b-row>
        <b-col cols="8" class=""> 

		  <div id="wrapper" class="wp" ref="wrapper">
		    <ul>
		      <li class="item" v-for="i in max" :key="i" v-waypoint="{ active: true, callback: onWaypoint, options: intersectionOptions}" :id="'sa-'+i" :ankerid="i">{{i}}</li> 
		    </ul>
		 
		  </div>
	   	</b-col>
        <b-col>
        	
			  <div class="container">
			    <h1>Vertical</h1>
			    <vue-slider   
			    tooltip="always" 
			    :min= "1"
			    :max= "max"
			    :interval="1"  
			    height="320px" 
			    class="star-slider" 
			    width="4" 
			    v-model="val"
			    :reverse="true"  
			    @callback="scrollTo"
			    @drag-start="isDragging=true" 
			    @drag-end="isDragging=false"   
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
        	<h2>w: {{isWayPointActive}}</h2>
        	<h2>d: {{isDragging}}</h2> 
	   		<a href="#" @click="setVal(--val);scrollTo();">&lt;</a>
	   		<a href="#" @click="setVal(++val);scrollTo();">&gt;</a>
	   	</b-col>
	    </b-row>
 

	</b-container> 

</div>
</template>

<script> 
import vueSlider from 'vue-slider-component'
import VueWaypoint from 'vue-waypoint'
import VueScrollTo from 'vue-scrollto'
import VueLodash from 'vue-lodash'

const options = { name: '_' } 

import Vue from 'vue'
 
Vue.use(VueWaypoint)
Vue.use(VueScrollTo)
Vue.use(VueLodash, options) // options is optional

export default {
  components: {
    vueSlider
  },
  name: 'app', 
  methods: {
  	setValRaw(val) { 
  		this.val = val;
	    console.log('-->setValRaw',  this.val )
  	},
    scrollToRaw() {    

		console.info("SCROLLTO", this.val);

		let options = {
		    container: '#wrapper', 
		    cancelable: false,
		    offset: -10,

		    onStart: (element) => {
		      this.isWayPointActive=false;
		    },
		    onDone: (element) => {
		      this.isWayPointActive=true;
				console.info("SCROLLTO END");
		    },
		    onCancel: function() {  
		    },
		    x: false,
		    y: true
		}

		VueScrollTo.scrollTo(document.getElementById('sa-'+this.val), 100, options) 
    },
    onWaypoint (a) {   
    		if (this.isDragging) return;
    		if (!this.isWayPointActive) return;

               console.log(a.direction)  

              if (a.direction === this.$waypointMap.DIRECTION_TOP) {
	              if (a.going === this.$waypointMap.GOING_OUT) { 
	                let val = parseInt(a.el.getAttribute("ankerid"))
	                console.log('GOING_OUT!', val) 
	                this.setVal(val +1)
	                console.log('-->next', val, this.val )
	              }
              }


              if (a.direction === this.$waypointMap.DIRECTION_BOTTOM) {
	              if (a.going === this.$waypointMap.GOING_IN) {  
	                this.setVal(parseInt(a.el.getAttribute("ankerid")))
	                console.log('GOING_IN!', this.val)
	                console.log('-->next', this.val )
	              }
              }

    }, 
  },
  data() {  
    return { 
		isDragging: false,
		isWayPointActive: true,	 
		max: 111,
		val: 1,
		intersectionOptions: {
		  root: this.$refs.wrapper,
		  rootMargin: '0px',
		  thresholds: 1
		},
	}
  },
  created() { 
 		this.setVal = this._.debounce(this.setValRaw, 101) 
 		this.scrollTo= this._.debounce(this.scrollToRaw, 101) 
  },
  mounted() {    
  }
}
</script>

<style> 
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
