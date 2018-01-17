<template>
<div class="wrapper">
	
</div>
</template>

<script>
import Vuex from 'vuex'
import * as WHS from 'whs'
import {Vector3} from "three"
import * as THREE from 'three'
// import {MeshPhongMaterial} from 'three'
// import {App} from '@whs/core/App';
// import { World } from 'whs/lib/core/World';
// import { Sphere } from 'whs/lib/components/meshes/Sphere';
// import { Plane } from 'whs/lib/components/meshes/Plane';
// const WHS = require('whs')
// import {App} from '@whs/core/App';

export default {

  name: 'whs',
  props: ['mapCenter'],
  components: {},
  data () {
    return {

    }
  },
  mounted(){
  	const app = new WHS.App([
  	  new WHS.ElementModule(), // Apply to DOM.
  	  new WHS.SceneModule(), // Create a new THREE.Scene and set it to app.

  	  new WHS.DefineModule('camera', new WHS.PerspectiveCamera({ // Apply a camera.
        position: new Vector3(0, 0, 50)
  	  })),

  	  new WHS.RenderingModule({bgColor: '#C85E5E'}), // Apply THREE.WebGLRenderer
  	  new WHS.ResizeModule() // Make it resizable.
  	])

  	// Sphere
  	const sphere = new WHS.Sphere({ // Create sphere comonent.
  	  geometry: {
  	    radius: 5,
  	    widthSegments: 32,
  	    heightSegments: 32
  	  },

  	  material: new THREE.MeshPhongMaterial({
  	    color: 0xF2F2F2
  	  }),

  	  position: new THREE.Vector3(0, 5, 0)
  	});

  	sphere.addTo(app);

  	// Plane
  	new WHS.Plane({
  	  geometry: {
  	    width: 100,
  	    height: 100
  	  },

  	  material: new THREE.MeshPhongMaterial({color: 0x447F8B}),

  	  rotation: {
  	    x: -Math.PI / 2
  	  }
  	}).addTo(app);

  	// Lights
  	new WHS.PointLight({
  	  light: {
  	    intensity: 0.5,
  	    distance: 100
  	  },

  	  shadow: {
  	    fov: 90
  	  },

  	  position: new THREE.Vector3(0, 10, 10)
  	}).addTo(app);

  	new WHS.AmbientLight({
  	  light: {
  	    intensity: 0.4
  	  }
  	}).addTo(app);

  	app.start() // Run app.

  },
  computed:{
	...Vuex.mapGetters([]),

  },
  watch:{

  },
  methods:{
	...Vuex.mapMutations([]), 
	...Vuex.mapActions([]), 

  },
}
</script>

<style lang="css" scoped>

.wrapper{

}
</style>