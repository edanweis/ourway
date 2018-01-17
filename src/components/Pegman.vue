<template>
	<div class="wrapper">
		<div class="pegman">

		</div>
	</div>	
</template>

<script>
import Vuex from 'vuex'

export default {

  name: 'pegman',
  props: [''],
  components: {},
  data () {
    return {

    }
  },
  created(){

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

<style lang="scss" scoped>
/* I saw a pen that was a js version and wanted to do a pure css
https://codepen.io/euan/pen/GZpBrX
*/

body {
  background-color: #333;
}

@mixin animation($animate...) {
    $max: length($animate);
    $animations: '';

    @for $i from 1 through $max {
        $animations: #{$animations + nth($animate, $i)};

        @if $i < $max {
            $animations: #{$animations + ", "};
        }
    }
    -webkit-animation: $animations;
    -moz-animation:    $animations;
    -o-animation:      $animations;
    animation:         $animations;
}

@mixin keyframes($animationName) {
    @-webkit-keyframes #{$animationName} {
        @content;
    }
    @-moz-keyframes #{$animationName} {
        @content;
    }
    @-o-keyframes #{$animationName} {
        @content;
    }
    @keyframes #{$animationName} {
        @content;
    }
}

.wrapper{
	// top: 0;
	// left: 0;
	// width: 100vw;
	// height: 100vh;
	// display: flex;
	// align-items: center;
	// justify-content: center;
	// background-color: red;
}

.pegman {
  z-index: 1;
  width: 98px;
  height: 104px;
  // margin-left: -17px;
  // margin-top: -52px;
  // margin: 5em auto;
  display: block;
  background: transparent url('https://maps.gstatic.com/tactile/pegman/default/pegman-2x.png') 0 0 no-repeat;
  @include animation('spinLink .8s steps(16) infinite');
}
@include keyframes(spinLink) {
  0%   { background-position: 0 0; }
  100% { background-position: 0 -1664px;}
}

</style>