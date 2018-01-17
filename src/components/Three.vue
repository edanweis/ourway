<template>
<div class="wrapper">
	<div ref="container"></div>
	<canvas id="canvas"></canvas>
</div>
</template>

<script>
import Vuex from 'vuex'
var THREE = require('three')

export default {

  name: 'three',
  props: ['mapCenter'],
  components: {

  },
  data () {
    return {
    	camera: null,
    	scene: null,
    	renderer: null,
    	geometry: null,
    	material: null,
    	mesh: null,
    }
  },
  mounted(){

  	this.camera = new THREE.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 0.01, 10 );
  	this.camera.position.z = 1;

  	this.scene = new THREE.Scene();

  	this.geometry = new THREE.BoxGeometry( 0.2, 0.2, 0.2 );
  	this.material = new THREE.MeshNormalMaterial();

  	this.mesh = new THREE.Mesh( this.geometry, this.material );
  	this.scene.add( this.mesh );

  	this.renderer = new THREE.WebGLRenderer( { antialias: true } );
  	this.renderer.setSize( window.innerWidth, window.innerHeight );
  	this.$refs.container.appendChild( this.renderer.domElement );

  	this.animate()
  },
  computed:{
	...Vuex.mapGetters([]),

  },
  watch:{

  },
  methods:{
	...Vuex.mapMutations([]), 
	...Vuex.mapActions([]), 

	animate: function(){
		  // window.requestAnimationFrame(this.animate.bind(this), this.$els.canvas)
	    window.requestAnimationFrame( this.animate() );
	    this.mesh.rotation.x += 0.01;
	    this.mesh.rotation.y += 0.02;
	    this.renderer.render( this.scene, this.camera );		
	}

  },
}
</script>

<style lang="css" scoped>


</style>