<template>
<div class="marker-wrapper">
	<!-- {{feature}} -->
</div>
</template>

<script>
import Vuex from 'vuex'

export default {

  name: 'Marker',
  props: ['feature'],
  components: {},
  data () {
    return {
      
    }
  },
  created(){
    // console.log(this.feature)

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

<style scoped>
.marker-wrapper{
  position: absolute;
  left: -25px;
  top:-25px;
	width:50px;
	height:50px;
	background-color: white;
	border-radius: 50%;
  border: 3px solid black;
}
</style>