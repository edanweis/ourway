<template>
  <v-app id="app">  
  <v-content>
    <v-container fill-height>
      <home/>
    </v-container>
  </v-content>
  </v-app>
</template>

<script>
import Home from './components/Home'

export default {
  name: 'app',
  components: {
    Home
  }
}
</script>

<style lang="css">

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0; 
  padding: 0;
}

html, body{
  // overflow: hidden !important;
  margin: 0; 
  padding: 0;
}

</style>
