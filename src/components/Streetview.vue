<template>
<div class="wrapper">
	<!-- <a @click="toggleStreetView">toggle</a> -->
	<!-- <a @click="attachEl">attach</a> -->

	<div ref="streetviewel" id="map"> </div>
</div>
</template>

<script>
import Vue from 'vue'
import Vuex from 'vuex'
import StreetviewMarker from "./StreetviewMarker";
export default {

  name: 'streetview',
  props: ['position'],
  components: {
  	StreetviewMarker
  },
  data () {
    return {

    }
  },
  mounted(){

  	var panorama;

     	var self = this
       var astorPlace = this.position

       // Set up the map
       var map = new google.maps.Map(self.$refs.streetviewel, {
         center: astorPlace,
         zoom: 18,
         streetViewControl: false
       });

       // Set up the markers on the map
       var cafeMarker = new google.maps.Marker({
           position: {lat: 40.730031, lng: -73.991428},
           map: map,
           icon: 'https://chart.apis.google.com/chart?chst=d_map_pin_icon&chld=cafe|FFFF00',
           title: 'Cafe'
       });

       var bankMarker = new google.maps.Marker({
           position: {lat: 40.729681, lng: -73.991138},
           map: map,
           icon: 'https://chart.apis.google.com/chart?chst=d_map_pin_icon&chld=dollar|FFFF00',
           title: 'Bank'
       });

       var busMarker = new google.maps.Marker({
           position: {lat: 40.729559, lng: -73.990741},
           map: map,
           icon: 'https://chart.apis.google.com/chart?chst=d_map_pin_icon&chld=bus|FFFF00',
           title: 'Bus Stop'
       });

       // We get the map's default panorama and set up some defaults.
       // Note that we don't yet set it visible.
       panorama = map.getStreetView();
       panorama.setPosition(astorPlace);
       panorama.setPov(/** @type {google.maps.StreetViewPov} */({
         heading: 265,
         pitch: 0
       }));
       panorama.linksControl = false
       panorama.panControl = false
       panorama.enableCloseButton = false
       

     panorama.setVisible(true);

     // this.attachEl()
  },
  computed:{
	...Vuex.mapGetters([]),

  },
  watch:{

  },
  methods:{
	...Vuex.mapMutations([]), 
	...Vuex.mapActions([]), 

	toggleStreetView() {
	  var toggle = panorama.getVisible();
	  if (toggle == false) {
	    panorama.setVisible(true);
	  } else {
	    panorama.setVisible(false);
	  }
	},

	attachEl: function(){
		// var child = document.getElementsByClassName('gmnoprint')[0]
		var child = document.querySelectorAll('[title="Cafe"]')[0]
		console.log(child)
		const makerContent = Vue.extend(StreetviewMarker, {
	        paramAttributes: ['feature']
	      })	
		new makerContent({
		  // store: store,
		  // data: {
		  //   feature: feature
		  // },
		  // propsData: {
		    
		  // }
		}).$mount(child); 

	},



  },
}
</script>

<style lang="css" scoped>

#map {
	position: absolute;
	top: 0px;
	right: 0px;
	width: 500px;
    height: 300px;
    background-color: white;
      }

    .wrapper{
    	
    }
      
</style>