<template>
  <div id="app">
    
    
    <Playlist/>
  </div>
</template>

<script>

import Playlist from './components/Playlist.vue'


export default {
  name: 'App',
  components: {
    Playlist
  }
}
</script>

<style>
html {
  min-height: 100%;
}
body {

  color: #fff;
  background: linear-gradient(#191414, #242020 80%);
  
}

#app {
  
  color: #fff;

  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  
  


}
</style>
