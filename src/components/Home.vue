<template>
<div class="wrapper">
  <div class="left">
  <mappy class="mmap" container="map" :zoom="mapZoom" :LngLat="[144.9746203,-37.82610754]" mapStyle="" :features="features" @loaded="loaded" :token="token" @move="mmapMove" @zoom="mmapZoom" @moveend="mmapMoveEnd"></mappy> 
  </div>
  <div class="right">
    <!-- <gmap-map class="gmap" :center="mapCenter" :zoom="mapZoom+1" :bearing="mapBearing" map-type-id="terrain"></gmap-map> -->

  <!-- <transition name="fade" appear> -->
    <!-- <gmap-street-view-panorama v-if="!moving" class="pano" :position="panCenter"
       :zoom="1">
    </gmap-street-view-panorama> -->

    <gsvpano :mapCenter="mapCenter">
      
    </gsvpano>

    <!-- <streetview v-if="sv" :position="{lat: 40.729884, lng: -73.990988}"></streetview> -->

  <!-- </transition> -->
    </div>

</div>
</template>

<script>
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
import mapState from 'vuex'
import Mappy from './Mappy'
import Streetview from './Streetview'
import Gsvpano from './Gsvpano'
import {db} from '../firebase-db.js'

const credentials = require('../credentials.js')

export default {

  name: 'HelloWorld',
  components: {
    Mappy,
    Streetview,
    Gsvpano
  },
  data () {
    return {
      mapCenter: {lat: 37.869260, lng: -122.254811},
      panCenter:  {lat: 37.869260, lng: -122.254811},
      mapZoom: 17,
      mapBearing: 0,
      pov: {heading: 165, pitch: 0},
      pano: null,
      token: credentials.mapbox.token,
      moving: false,
      sv: false,
      markers: [{
        position: {lat: 37.869260, lng: -122.254811}
      }, {
        position: {lat: 37.869360, lng: -122.254911},
      }]
    }
  },
  created(){
    var self = this
    var data = db.ref('data/features')

    // We call a dispatch on the Vuex action so we can resolve the promise.
    this.$store.dispatch('setDataRef', data ).then(response => {
      // do something here to handle async loading if necessary.
    })


  },
  computed:{
    // not sure why Vuex.mapState isn't working.
  // ...Vuex.mapState(['features']),
  ...Vuex.mapGetters(['features', 'map']),

  


  },
  watch:{

  },
  methods:{
  ...Vuex.mapMutations(['setMap']), 
  ...Vuex.mapActions([]), 

  loaded: function(map){
    this.setMap(map)
    this.map.dragRotate.disable()
    this.map.touchZoomRotate.disableRotation()
  },

  mmapMove: function(center){
    this.moving = true
    this.mapCenter = center
    this.mapBearing = this.map.getBearing()
    this.panCenter = {'lat': this.mapCenter.lat, 'lng': this.mapCenter.lng}
    // this.panCenter = {lat: 37.869260, lng: -122.254811}
    console.log(this.panCenter)

  },
  
  mmapMoveEnd: function(){
    this.moving = false
    this.sv = true
  },
  mmapZoom: function(level){
    this.mapZoom = Math.round(level)
  },

  updatePov(pov) {
    this.pov = pov;
  },
  updatePano(pano) {
    this.pano = pano;
  }

  },
}
</script>

<style lang="css" scoped>

.wrapper{
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden !important;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.mmap{
  position: relative;
  display: block;
  width: 50% !important;
  height: 100vh;
  padding: 0;
  margin: 0;
}
.gmap{
  /*width: 50% !important;*/
  /*left: 50vw;*/
  position: relative !important ;
  display: block;
  padding: 0;
  margin: 0;
  height: 100vh;
}

.pano{
/*position: absolute;*/
/*display: block;*/
/*bottom: 100px;*/
height: 50vh;
width: 50vw;
padding: 0;
margin: 0;
}

.left{
  width: 50vw;
  height: 100vh;
}

.right{
  width: 50vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.fade-enter-active, .fade-leave-active {
  transition: all 540ms cubic-bezier(0,.45,.45,1) 500ms
}
.fade-enter, .fade-leave-active {
  /*opacity: 0;*/
  filter: grayscale(100%) brightness(30%); 
  /*scale: 0.5;*/
}


</style>