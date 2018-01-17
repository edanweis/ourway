<template>
  <v-layout row wrap align-center justify-center>
    <v-flex xs12 style="min-height: 500px;">
      <gmap-street-view-panorama ref="streetview" :motionTracking="true" key="gmap_sv" class="streetview" :position="mapCenter" @pov_changed="updatePov" @pano_changed="updatePano" :options="{addressControl: false, fullscreenControl: false,scrollwheel:false, zoomControl: true, mode:'html4'}" @zoom="updateZoom()">
      </gmap-street-view-panorama>
      <aframe key="aframe" :userHeight="userHeight" v-if="panoSrc" class="aframe" :panoSrc="panoSrc" :fov="fov" :mapCenter="mapCenter" :pov="pov" @camera="cameraChange()" :rotationOffset="rotationOffset" :positionOffset="positionOffset" :sphereRadius="sphereRadius"></aframe>
      <gsvpano v-if="gsvpano" key="gspano" class="gsvpano" :mapCenter="mapCenter" @output="getImage"></gsvpano>
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
import Gsvpano from './Gsvpano'
import aframe from './aframe'

const credentials = require('../credentials.js')

export default {

  name: 'HelloWorld',
  components: {
    Gsvpano,
    aframe,
  },
  data () {
    return {
      gsvpano: false,
      userHeight: 0.7,
      sphereRadius: 195.5,
      rotationOffset: {x: -2, y: -11, z: -1},
      positionOffset: {x: 0, y: 0, z: 0},
      mapCenter: {lat: -37.8756663, lng: 144.9761177},
      panCenter: {lat: -37.8756663, lng: 144.9761177},
      fov: 66,
      zoom: 1,
      rotation: null,
      pov: null,
      pano: null,
      panoSrc: null,
    }
  },
  methods:{
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
    console.log('updated zoom')
  },
  cameraChange(event){
    this.cameraAngle = event
  },

  updatePano(pano) {
    this.pano = pano;
  }

  },
}
</script>

<style lang="css" scoped>

.aframe{
  z-index: 2;
  position: absolute;
  width: 900px;
  height: 500px;
  padding: 0;
  margin: 0;
  pointer-events: none;
  opacity: 0.5;
}
.streetview {
  position: absolute;
  width: 900px;
  height: 500px;
  margin: 0; 
  padding: 0;
  z-index: 1;

}
.pano{
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
  display: flex;
  flex-direction: row;
}
</style>