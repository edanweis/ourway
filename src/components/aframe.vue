	<template>
	<div class="wrapper">
		<a-scene antialias="true" embedded>
			<a-assets>
				<img ref="sky" id="sky" :src="panoSrc" crossorigin="anonymous">
				 
			</a-assets>
		   <a-cylinder position="1.3 0 -2" radius="0.02" height="1.5" color="#FFC65D"></a-cylinder>

		   <a-box event-set__1="_event: dragstart; material.opacity: 0.2" position="1.3 .75 -2" rotation="0 0 0" color="#4CC3D9"  depth=".05" height=".5" width="0.4">
		   	<a-animation attribute="material.opacity" begin="dragstart" from="1.0" to="0.2" dur="200"></a-animation>
		   	<a-animation attribute="material.opacity" begin="dragend" from="0.2" to="1.0" dur="200"></a-animation>
		   </a-box>
		   <a-entity gltf-model="url(static/align.gltf)"></a-entity>
		  <a-sky id="image-360" :radius="sphereRadius" src="#sky" rotation="0 0 0"></a-sky>
		  <a-camera listener ref="camera" :user-height="userHeight" look-controls="reverseMouseDrag: true" :fov="fov" :rotation="rotation" :position="position"></a-camera>
		    <!-- <a-entity io3d-furniture="id:0b35a7a1-1251-4652-bfdb-62375558db99" position="0 0 0"></a-entity> -->
		  <!-- <a-camera look-controls-enabled="false" keyboard-controls="mode: fps"></a-camera> -->
		</a-scene>

	</div>
	</template>

	<script>
	var AFRAME = require('aframe')
	import Vue from 'vue'
	import Vuex from 'vuex'
	// require('aframe-drag-look-controls-component')

	// import registerClickDrag from 'aframe-click-drag-component';
	// registerClickDrag(AFRAME);

	// AFRAME.registerComponent('aframe-click-drag-component', {
	// 	  init: function () {
	// 	    console.log('Hello, World!');
	// 	  }
	// 	});


	// AFRAME.registerComponent('listener', {
	//   tick: function (self) {
	//     var r = this.el.getAttribute('rotation')
	//     self.$emit('cameraRotation', r)
	//   }
	// });

	export default {
	  name: '',
	  props: ['mapCenter', 'panoSrc', 'pov', 'fov', 'userHeight', 'rotateScene', 'rotationOffset', 'positionOffset', 'sphereRadius'],
	  components: {},
	  data () {
	    return {	
	    	zoom: 0,
	    }
	  },
	  mounted(){	

	  },
	  computed:{
		...Vuex.mapGetters([]),

		rotation: function(){
			return {x: this.pov.pitch + this.rotationOffset.x, y: -1*this.pov.heading + this.rotationOffset.y, z: 0 + this.rotationOffset.z}
		},

		position: function(){
			return {x: this.positionOffset.x, y: this.positionOffset.y, z: this.positionOffset.z}
		},

	  },
	  watch:{

	  	// panoSrc: function(val){
	  		// this.$refs.sky.setAttribute('img', 'src', val);
	  		// console.log('pano changed')
	  		// var self = this
	  		// setTimeout(function () {
	  			
	  		// }.bind(this), 1000)
	  		// this.$refs.camera.addEventListener('componentchanged', function (event) {
	  		//   this.$emit('camera', event)
	  		// });
	  	// }
	  },
	  methods:{
		...Vuex.mapMutations([]), 
		...Vuex.mapActions([]), 

	  },
	}
	</script>

	<style lang="css" scoped>

	.wrapper{
		position: relative;
		top: 0;
		left: 0;

		
	}
	a-scene{
		display: block;
		position: absolute;
		left: 0;
		top: 0;

		/*width: 100%;
		height: 100%;*/
		/*border: 1px solid blue;*/
		/*height: 50vh;*/
		/*top: 0px;*/
	}
	
	</style>