<template>
<div class="wrapper">
	<div id="pano" ref="panoBox" style="display: inline"></div>
</div>
</template>

<script>
import Vuex from 'vuex'
var GSVPANO = require('gsvpano')
var _ = require('lodash')
// import GSVPANO from 'gsvpano'

export default {

  name: 'Gsvpano',
  props: ['mapCenter'],
  components: {},
  data () {
    return {
    	// canvas: null
    	zoom: 1
    }
  },
  mounted(){
	 

  },
  computed:{
	...Vuex.mapGetters([]),

  },
  watch:{

  	mapCenter: _.debounce(function(mapCenter){
  		if(mapCenter){
	  	   var self = this	
	  	   console.log(mapCenter)
		   var lat = mapCenter.lat
		   var lng = mapCenter.lng
		   var panoBox = this.$refs.panoBox
		    
		   // Create a loader.
		   var loader = new GSVPANO.PanoLoader({
		     zoom: self.zoom
		   })

		   
		   // Implement the onPanoramaLoad handler
		   loader.on('panorama.load', function(pano) {
		   	console.log('loading...')
		     var canvas = self.$refs.panoBox
		   })

		   loader.on('panorama.data', function(pano) {
		   	console.log('getting data')
		     var canvas = pano.canvas
		     while (panoBox.firstChild) {
		       panoBox.removeChild(panoBox.firstChild)
		     }
		     panoBox.appendChild(canvas)
		   })

		   // Invoke the load method with a LatLng point.
		   loader.load(new google.maps.LatLng(lat, lng))

		   // Set error handle.
		   loader.on('error', function(message) {
		     console.log(message)
		   })
  		}
  	}, 500),

  },
  methods:{
	...Vuex.mapMutations([]), 
	...Vuex.mapActions([]), 

	completeCallback: function(){
		console.log('completee!')
		var panoBox = this.$refs.panoBox
		while (panoBox.firstChild) {
         panoBox.removeChild(panoBox.firstChild)
       }
       panoBox.appendChild(this.canvas)
	},

	changeProgress: function(){
		console.log('changing...')
	}

  },
}
</script>

<style lang="css" scoped>

.wrapper{
	/*background-color: blue*/
}

</style>