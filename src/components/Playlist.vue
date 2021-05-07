<template>
    <div id="Playlist">
        <h1>Playlist Analyzer</h1>
        <p>{{test}}</p>
        <p><span v-html="data.noauth"></span></p>
        
        <button @click="count++">+1</button>
        <p>{{count}}</p>

        <form @submit.prevent="getPlaylist">
            <div id="form">
                <input type="text" v-model.trim="searchURL" name="search" id="searchfield" placeholder="HTTP address to playlist">
            </div>
        </form>

        
        <ul>
            <li v-for="track in data" :key="track.track.name">
                {{track.track.name}} - {{track.track.popularity}}
            </li>
        </ul>
    </div>

</template>

<script>

import axios from 'axios';

export default {
  name: 'Playlist',
  data: function () {
    return {
      test: 'hello test',
      count: 0,
      data: {},
      searchURL: '',
      noauth: ''
      
    }
  },
  props: {
    
  },
  methods: {
    getPlaylist: function () {
        let bodyFormData = new FormData();
        bodyFormData.append('search', this.searchURL);
        
        axios({
            method: 'post',
            url: 'http://localhost:3000/search',
            data: {search: this.searchURL},
            headers: { 'Content-Type': 'application/json'},
        })
        .then(response => {
            console.log(response.status);
            console.log(response.data);
            this.data = response.data;
        })
        .catch(error => console.error('axios error: ' + error))
    }
  
  }
}
</script>

<style scoped>
p {

color: #000080;
}

#form {
width: 35%;
margin-left: auto;
margin-right: auto;
}    

li {    
text-align: left;
}

</style>

