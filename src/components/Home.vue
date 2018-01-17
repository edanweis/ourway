<template>
  <v-layout row wrap align-center justify-center>
    <v-flex xs12 style="min-height: 500px;">

      <!-- <div class="wrapper"> -->
        <!-- <pegman class="pegman"></pegman> -->
        <!-- <div class="container"> -->
          <!-- <div class="left"> -->
            <!-- <mappy class="mmap" container="map" :zoom="mapZoom" :LngLat="[144.9746203,-37.82610754]" mapStyle="" :features="features" @loaded="loaded" :token="token" @move="mmapMove" @zoom="mmapZoom" @moveend="mmapMoveEnd"></mappy>  -->
            <!-- <streetview class="streetview" v-if="sv && mapCenter" :position="mapCenter"></streetview> -->
            <!-- <transition-group name="fade" appear> -->

              <gmap-street-view-panorama ref="streetview" :motionTracking="true" key="gmap_sv" class="streetview" :position="mapCenter" @pov_changed="updatePov" @pano_changed="updatePano" :options="{addressControl: false, fullscreenControl: false,scrollwheel:false, zoomControl: true, mode:'html4'}" @zoom="updateZoom()">
              </gmap-street-view-panorama>
              <aframe key="aframe" :userHeight="userHeight" v-if="panoSrc" class="aframe" :panoSrc="panoSrc" :fov="fov" :mapCenter="mapCenter" :pov="pov" @camera="cameraChange()" :rotationOffset="rotationOffset" :positionOffset="positionOffset" :sphereRadius="sphereRadius"></aframe>
              <gsvpano v-if="gsvpano" key="gspano" class="gsvpano" :mapCenter="mapCenter" @output="getImage"></gsvpano>

              <!-- </transition-group> -->
              <!-- </div> -->
              <!-- <div class="right"> -->
                <!-- <gmap-map class="gmap" :center=
                  "mapCenter" :zoom="mapZoom+1" :bearing="mapBearing" map-type-id="terrain"></gmap-map> -->
      <!-- <transition name="fade" appear>
      </transition> -->
      <!-- </div> -->
    </v-flex>
    
    <v-flex xs12>
      
       <ul>
        <span>Heading: {{pov && pov.heading}}</span>
        <span> Pitch: {{pov && pov.pitch}}</span>
        <span> Zoom: {{zoom}}</span>
        <v-slider style="padding: 0;" thumb-label :label="'FOV '+fov" :max="180" v-model="fov"></v-slider>      
        <v-slider style="padding: 0;" thumb-label :label="'User Height '+ userHeight" step="0.1" :max="8.0" :min="-4.0" v-model="userHeight"></v-slider>
        <v-slider style="padding: 0;" thumb-label :label="'Sphere Radius '+ sphereRadius" step=".5" :min="1" :max="200" v-model="sphereRadius"></v-slider>
        <v-slider style="padding: 0;" thumb-label :label="'rotationOffsetX '+ rotationOffset.x" step="1" :max="20.0" :min="-20.0" v-model="rotationOffset.x"></v-slider>
        <v-slider style="padding: 0;" thumb-label :label="'rotationOffsetY '+ rotationOffset.y" step="1" :max="20.0" :min="-20.0" v-model="rotationOffset.y"></v-slider>
        <v-slider style="padding: 0;" thumb-label :label="'rotationOffsetZ '+ rotationOffset.z" step="1" :max="20.0" :min="-20.0" v-model="rotationOffset.z"></v-slider>

        <v-slider style="padding: 0;" thumb-label :label="'positionOffsetX '+ positionOffset.x" step="1" :max="20.0" :min="-20.0" v-model="positionOffset.x"></v-slider>
        <v-slider style="padding: 0;" thumb-label :label="'positionOffsetY '+ positionOffset.y" step="1" :max="20.0" :min="-20.0" v-model="positionOffset.y"></v-slider>
        <v-slider style="padding: 0;" thumb-label :label="'positionOffsetZ '+ positionOffset.z" step="1" :max="20.0" :min="-20.0" v-model="positionOffset.z"></v-slider>
    </ul>
  
</v-flex>
</v-layout>
</template>

<script>
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
import mapState from 'vuex'
import Mappy from './Mappy'
import Streetview from './Streetview'
import Pegman from './Pegman'
import Three from './Three'
import Whs from './Whs'
import Scene from './Scene'
import Gsvpano from './Gsvpano'
import aframe from './aframe'
import {db} from '../firebase-db.js'


const credentials = require('../credentials.js')

export default {

  name: 'HelloWorld',
  components: {
    Mappy,
    Streetview,
    Gsvpano,
    Scene,
    Three,
    Whs,
    aframe,
    Pegman
  },
  data () {
    return {
      gsvpano: false,
      userHeight: 0.7,
      sphereRadius: 195.5,
      rotationOffset: {x: -2, y: -11, z: -1},
      positionOffset: {x: 0, y: 0, z: 0},
      fov: 66,
      zoom: 1,
      rotation: null,
      mapCenter: {lat: -37.8756663, lng: 144.9761177},
      panCenter:  {lat: -37.8756663, lng: 144.9761177},
      mapZoom: 17,
      mapBearing: 0,
      pov: null,
      pano: null,
      token: credentials.mapbox.token,
      moving: false,
      sv: false,
      panoSrc: null,
      markers: [{
        position: {lat: -37.8756663, lng: 144.9761177},
      }, {
        position: {lat: -37.8756663, lng: 144.9761177},
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

  cameraRotation(r){
    this.rotation = r
  },

  fov_calc: function(){
    return ( 180 / Math.pow(2,this.zoom) )
  },

  getImage: function(image){
    this.panoSrc = image
  },
  updatePov(pov) {
    this.gsvpano = true
    this.pov = pov
  },
  updateZoom(zoom) {
    // this.zoom = zoom
    console.log('updated zoom')
    // console.log(this.$refs.streetview)
  },

  loaded: function(map){
    this.setMap(map)
    this.map.dragRotate.disable()
    this.map.touchZoomRotate.disableRotation()
  },

  cameraChange(event){
    this.cameraAngle = event
  },

  mmapMove: function(center){
    this.moving = true
    this.mapCenter = center
    this.panoSrc = null
    // this.mapBearing = this.map.getBearing()
    this.panCenter = {'lat': this.mapCenter.lat, 'lng': this.mapCenter.lng}
    // this.panCenter = {lat: 37.869260, lng: -122.254811}
    // console.log(this.panCenter)

  },
  
  mmapMoveEnd: function(){
    this.moving = false
    this.sv = true
  },
  mmapZoom: function(level){
    this.mapZoom = Math.round(level)
  },

  updatePano(pano) {
    this.pano = pano;
  }

  },
}
</script>

<style lang="stylus" scoped>
@import '../stylus/main'

.pegman{
position: absolute;
left: 50%;
top: 50%;
margin-left: -17px;
margin-top: -52px;

width: 100vw;
height: 104px !important;
}

.gsvpano{
  // display: none;
}
.aframe{
  z-index: 2;
  position: absolute;
  width: 900px;
  height: 500px;
  padding: 0;
  margin: 0;
  pointer-events: none;
  opacity: 0.5;
  // border: 1px solid red;
}
.streetview {
  position: absolute;
  width: 900px;
  height: 500px;
  margin: 0; 
  padding: 0;
  z-index: 1;

}
.mmap{
  position: relative;
  display: block;
  width: 100% !important;
  /*height: 100vh;*/
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
/*height: 50vh;*/
width: 50%;
padding: 0;
margin: 0;
}

.left{
  width: 50vw;
  height: 100vh;
}

.right{
  width: 50vw;
  /*height: 100vh;*/
  display: flex;
  flex-direction: row;
}

.fade-enter-active, .fade-leave-active {
  transition: all 240ms cubic-bezier(0,.45,.45,1)
}
.fade-enter, .fade-leave-active {
  opacity: 0;
  /*filter: grayscale(100%) brightness(200%); */
  /*scale: 0;*/
}


</style>