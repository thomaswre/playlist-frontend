<template>
    <div id="Playlist">
        <h1>Playlist Analyzer</h1>
        
        <p v-if="data.noauth"><span v-html="data.noauth" id="noauth"></span></p>
    
        <form @submit.prevent="getPlaylist">
            <div id="form">
                <input type="text" v-model.trim="searchURL" name="search" id="searchfield" placeholder="HTTP address to playlist">
            </div>

        </form>

        <p v-if="errors.length">
            <ul>
                <li v-for="error in errors" :key="error" class="searchError">{{ error }}</li>
            </ul>
        </p>

        <p v-if="data.failed">
            <span v-html="data.failed" id="failed" class="searchError"></span>
        </p>

        <div v-if="data.length" id="data">
            <div id="playliststats">
                <h2>Track Averages</h2>
                
                <span>Popularity: {{Math.round(getAverage(this.features.popularity))}}%</span>
                <span>Danceability: {{Math.round(getAverage(this.features.danceability)*100)}}%</span>
                <span>Energy: {{Math.round(getAverage(this.features.energy)*100)}}%</span>
                <span>Valence: {{Math.round(getAverage(this.features.valence)*100)}}%</span>
                <span>BPM: {{Math.round(getAverage(this.features.tempo))}}</span>
            </div>
            
            <ul id="cards-view">
                <li class="trackcard" v-for="track in data" :key="track.track.id">
                    <h3>{{track.track.artists[0].name}}</h3> 
                    <h4>{{track.track.name}}</h4>
                    <img v-bind:src="'' + track.track.album.images[0].url" class="albumcover"/>

                    <transition-group tag="div" name="fillWidth" class="featurewrapper" appear>
                        <div class="poplabel" key="1">Popularity: {{track.track.popularity}}%</div>
                        <div class="feature pop" key="2" v-bind:style="{ width: track.track.popularity + '%'}"></div>

                        <div key="3">Danceability: {{Math.round(track.features.danceability*100)}}%</div>
                        <div key="4" class="feature dance" v-bind:style="{ width: track.features.danceability*100 + '%'}"></div>

                        <div key="5">Energy: {{Math.round(track.features.energy*100)}}%</div>
                        <div key="6" class="feature energy" v-bind:style="{ width: track.features.energy*100 + '%'}"></div>
                            
                        <div key="7">Valence: {{Math.round(track.features.valence*100)}}%</div>
                        <div key="8" class="feature valence" v-bind:style="{ width: (track.features.valence).toFixed(1)*100 + '%'}"></div>

                        
                    </transition-group>
                    <span class="tempo">BPM: {{(track.features.tempo).toFixed(0)}}</span>
                </li>
            </ul>
        </div>
        
    </div>

</template>

<script>

import axios from 'axios';

export default {
  name: 'Playlist',
  data: function () {
    return {
      errors: [],
      count: 0,
      features: {},
      data: {},
      searchURL: null,
      noauth: '',
      failed: null
      
    }
  },
  props: {
    
  },
   
  
  
  methods: {

    // Builds a Feature object and inserts it into the regualar track object for easier handling
    extractFeatures: function (){
        console.log('Inside Features function');
        
        // Sets a few propertys with the features and empty arrays as values.
        this.features = {
          popularity: [],
          danceability: [],
          energy: [],
          valence: [],
          tempo: []
        }

        // Loops through each track, picks out the features and populates the empty arrays in this.features
        for (let track of this.data) {

            this.features.popularity.push(track.track.popularity);
            this.features.danceability.push(track.features.danceability);
            this.features.energy.push(track.features.energy);
            this.features.valence.push(track.features.valence);
            this.features.tempo.push(track.features.tempo);

        }

        // console.log(this.features);
        

    },

    // Calcs the average of an arrays values
    getAverage: function (array) {
        
        const total = array.reduce((prevValue, currentValue) => 
            prevValue + currentValue);
        return total/array.length;

    },

    getPlaylist: function () {
        
        this.errors = [];
        if (!this.searchURL) {

            return this.errors.push("Searchfield cannot be empty");
            
        }
        
        

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
            
            // Extract all the features from each track into the features{} object.
            // Each feature property holds an array with each track's feature data
            
            this.extractFeatures();

        })
        
        .catch(error => console.error('axios error: ' + error))
    }

  
  }
}
</script>

<style>

@media (min-width: 320px) {

    #noauth {
        font-size: 1.2rem
    }

    a {
        font-size: 1.2rem;
        color: #e11210;
    
    }
    
    h1 {
        text-align: center;
        color: #1DB954;
        font-size: 2rem;

    }

    p {
        color: #fff;
        
    }


    #form {
        width: 60%;
        margin-left: auto;
        margin-right: auto;
        
    }    

    .searchError {
        color: red;
    }

    #searchfield {
        width: 100%;
    }

    #data {
        
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    
    }
    #playliststats {
        margin-top: 2rem;
        width: 80%;
        text-align: center;
    }

    #playliststats span {
        
        margin-top: 0rem;
        margin-bottom: 0rem;
        margin-right: 1rem;
        font-size: 1rem;
    }

    #playliststats h3 {
        font-size: 1.3rem;
    }
    #cards-view {
        width: 80%;
        display: flex;
        padding: 0px;
        margin: 0px;
        justify-content: center;
        
        flex-wrap: wrap;
        


    }

    .trackcard {    
        display: flex;
        flex-wrap: wrap;
        list-style: none;
        color: #fff;
        
        margin-right: 0rem;
        margin-top: 2rem;
        
        
        background-color: #28343d;

    }

    h3 {
        width: 100%;
        text-align: center;
        margin: 0.5rem 0rem 0rem 0rem;
        font-size: 0.9rem;
    }

    h4 {
        width: 100%;
        text-align: center;
        margin: 0.2rem 0rem 0.3rem 0rem;
        font-size: 0.8rem;
        
    }

    .albumcover {
        
        margin-bottom: 0px;
        padding-bottom: 0px;
        
        width: 40%;
        margin-right: 1rem;

    }

    .featurewrapper {
        
        width: 50%;
        
        
    
    }
    .feature {
        
        margin-top: 0rem;
        margin-bottom: 0.2rem;
        height: 0.3rem;
        
        transition: all 0.5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
        
        
        
    }

    .pop {
        background-color: #49b675 ;
        
    }

    .dance {
        background-color: #bfff00;
        
    }

    .energy {
        background-color: #ff0012;
        
        
    }

    .tempo {

        width: 100%;
        text-align: center;
        color: lightsteelblue;
    
        /* animation: pulse 1s linear infinite; */
        font-weight: bold;
        padding: 0;
        margin: 0;
        
        


    }

    .valence {
        background-color: #007fff;
        
    }


}

@media (min-width: 768px) {

    
    h1 {
        text-align: center;
        color: #1DB954;
        font-size: 3rem;

    }
    p {
        color: #fff;
        
    }

    #form {
        width: 35%;
        margin-left: auto;
        margin-right: auto;
        
    }    

    #searchfield {
        width: 100%;
        height: 1rem;
        font-size: 1rem;
    }

    #data {
        justify-content: center;
    
    }
    #playliststats {
        width: 70%;
        
    }

    #playliststats span {
        font-size: 1.2rem;
    }

    #cards-view {
        
        padding: 0px;
        
        
        width: 70%;
        

    }
    .trackcard {    
        
        position: relative;
        list-style: none;
        color: #fff;
        
        margin-right: 2rem;
        margin-top: 3rem;
        width: 190px;
        background-color: #28343d;

    }

    h3 {
        text-align: center;
        margin: 0.5rem 0rem 0rem 0rem;
    }

    h4 {
        text-align: center;
        margin: 0.2rem 0rem;
        font-size: 1rem;
    }


    .albumcover {
        margin-bottom: 0px;
        padding-bottom: 0px;
        height: 190px;
        width: 190px;

    }

    .featurewrapper {
        float: none;
        width: 100%;
    
    }
    .feature {
        
        margin-top: 0rem;
        margin-bottom: 0rem;
        height: 0.4rem;
        
        transition: all 0.5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
        
        
    }


    li div div:nth-child(odd) {

        background-color: #3b4c59;
    }


    .poplabel {
        margin-top: 0rem;

    }
    .pop {
        background-color: #49b675 ;
        
        
        
    }

    .dance {
        background-color: #bfff00;
        
    }

    .energy {
        background-color: #ff0012;
        
        
    }

    @-webkit-keyframes pulse {
    0% {
    -webkit-transform: scale(1, 1);
    }
    50% {
    -webkit-transform: scale(1.05, 1.05);
    }
    100% {
    -webkit-transform: scale(1, 1);
    };
    }



    .tempo {
        width: 7rem;
        background-color: #005499;
        position: absolute;
        top: 0px;
        right: 0px;
        
        transform: translate(10%, 30%) rotate(35deg);
        
        font-weight: bold;
        border-radius: 15px;
        
    }

    .valence {
        background-color: #007fff;
        
    }



    .fillWidth-enter-active {
        width: 0 !important;
        
        
    }


}


</style>

