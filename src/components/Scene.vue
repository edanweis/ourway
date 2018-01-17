<template>
<div class="wrapper">
  <div id="pano" ref="panoBox" style="display: none"></div>
  <div ref="container"></div>


</div>
</template>

<script>
import Vuex from 'vuex'
import Vue from 'vue'
var GSVPANO = require('gsvpano')
var _ = require('lodash')
var THREE = require('three')
// import GSVPANO from 'gsvpano'

export default {

  name: 'Scene',
  props: ['mapCenter'],
  components: {},
  data () {
    return {
      // canvas: null
      zoom: 1
    }
  },
  created(){
    


  },
  computed:{
  ...Vuex.mapGetters([]),

  },
  watch:{

    mapCenter: _.debounce(function(mapCenter){
      var self = this
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
         self.init(canvas)
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

  init: function(canvas){

    var controls, camera, scene, renderer;
    var cameraCube, sceneCube;
    var textureEquirec, textureCube, textureSphere;
    var cubeMesh, sphereMesh;
    var sphereMaterial;
    var refract;
      // CAMERAS

      camera = new THREE.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 1, 100000 )
      camera.position.set( 0, 0, 1000 )
      cameraCube = new THREE.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 1, 100000 )

      // controls = new THREE.OrbitControls( camera )
      // controls.minDistance = 500
      // controls.maxDistance = 2500

      // SCENE

      scene = new THREE.Scene()
      sceneCube = new THREE.Scene()

      // Lights

      var ambient = new THREE.AmbientLight( 0xffffff )
      scene.add( ambient )

      // Textures

      // var textureLoader = new THREE.TextureLoader()

      textureEquirec =  new THREE.Texture(canvas) //textureLoader.load( "textures/2294472375_24a3b8ef46_o.jpg" )
      textureEquirec.mapping = THREE.EquirectangularReflectionMapping
      textureEquirec.magFilter = THREE.LinearFilter
      textureEquirec.minFilter = THREE.LinearMipMapLinearFilter

      // textureSphere = textureLoader.load( "textures/metal.jpg" )
      // textureSphere.mapping = THREE.SphericalReflectionMapping

      // Materials

      var equirectShader = THREE.ShaderLib[ "equirect" ]

      var equirectMaterial = new THREE.ShaderMaterial( {
        fragmentShader: equirectShader.fragmentShader,
        vertexShader: equirectShader.vertexShader,
        uniforms: equirectShader.uniforms,
        depthWrite: false,
        side: THREE.BackSide
      } )

      equirectMaterial.uniforms[ "tEquirect" ].value = textureEquirec
      
      //


      textureCube = new THREE.CubeTextureLoader().load( 'http://math.hws.edu/eck/cs424/notes2013/images/16/park-cube-map.jpg' );
      textureCube.format = THREE.RGBFormat;
      textureCube.mapping = THREE.CubeReflectionMapping;

      var geometry = new THREE.SphereBufferGeometry( 400.0, 48, 24 )
      sphereMaterial = new THREE.MeshLambertMaterial( { envMap: textureCube } )
      sphereMesh = new THREE.Mesh( geometry, sphereMaterial )
      scene.add( sphereMesh )

      //

      renderer = new THREE.WebGLRenderer()
      renderer.autoClear = false
      renderer.setPixelRatio( window.devicePixelRatio )
      
      renderer.setFaceCulling( THREE.CullFaceNone )
      this.$refs.container.appendChild( renderer.domElement )
  
      camera.lookAt( scene.position )
      cameraCube.rotation.copy( camera.rotation )

      renderer.render( sceneCube, cameraCube )
      renderer.render( scene, camera )

    },
    

    animate: function() {
      window.requestAnimationFrame( animate )
      this.render()
    },

    render: function() {

      var timer = -0.0002 * Date.now()

      

    }



  
  

  },
}
</script>

<style lang="css" scoped>

.wrapper{
  /*background-color: blue*/
}

</style>