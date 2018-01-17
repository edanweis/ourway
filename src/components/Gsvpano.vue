<template>
<div class="wrapper">
	<div id="pano" ref="panoBox" style="display: none"></div>
</div>
</template>

<script>
import Vuex from 'vuex'
var GSVPANO = require('gsvpano')

export default {

  name: 'Gsvpano',
  props: ['mapCenter'],
  components: {},
  data () {
    return {
    	zoom: 3
    }
  },
  mounted(){
	 this.loadPano(this.mapCenter)
  },
  watch:{
  	mapCenter: _.debounce(function(mapCenter){
  		var self = this
  		if(mapCenter){
	  	  self.loadPano(mapCenter)
  		}
  	}, 500)
  },
  methods:{
	completeCallback: function(){
		var panoBox = this.$refs.panoBox
		while (panoBox.firstChild) {
         panoBox.removeChild(panoBox.firstChild)
       }
       panoBox.appendChild(this.canvas)
	},

	loadPano: function(mapCenter){
  	   var self = this	
	   var lat = mapCenter.lat
	   var lng = mapCenter.lng
	   var panoBox = this.$refs.panoBox
	   // Create a loader.
	   var loader = new GSVPANO.PanoLoader({
	     zoom: self.zoom
	   })
	   // Invoke the load method with a LatLng point.
	   // loader.load(new google.maps.LatLng(lat, lng))
	   loader.load(new google.maps.LatLng(lat, lng))

	   // Implement the onPanoramaLoad handler
	   loader.on('panorama.load', function(pano) {
	     var canvas = self.$refs.panoBox
	   })

	   loader.on('panorama.progress', function(progress) {
	     
	   })

	   loader.on('panorama.data', function(pano) {
	   	console.log('getting data')
	     var canvas = pano.canvas
	     while (panoBox.firstChild) {
	       panoBox.removeChild(panoBox.firstChild)
	     }
	     panoBox.appendChild(canvas)

	     pano.on('complete', function(){
	     	self.$emit('output', canvas.toDataURL())
	     })
	   })


	   // Set error handle.
	   loader.on('error', function(message) {
	     console.log(message)
	   })
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