<template>
<div class="wrapper">
	
</div>
</template>

<script>
import Vuex from 'vuex'

export default {
  name: 'StreetviewMarker',
  props: ['feature'],
  components: {},
  data () {
    return {
      // feature: dataFeature,
      // feature: feature,
      // feature: feature
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
.wrapper{
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