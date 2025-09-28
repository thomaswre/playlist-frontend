<template>
    <div id="Playlist">
        <h1>Playlist Analyzer</h1>
        
        <p v-if="data.noauth"><span v-html="data.noauth" id="noauth"></span></p>
    
        <form @submit.prevent="getPlaylist">
            <div id="form">
                <input type="text" v-model.trim="searchURL" name="search" id="searchfield" placeholder="HTTP address to playlist">
            </div>

        </form>

        <div v-if="errors.length">
            <ul>
                <li v-for="error in errors" :key="error" class="searchError">{{ error }}</li>
            </ul>
        </div>

        <p v-if="data.failed">
            <span v-html="data.failed" id="failed" class="searchError"></span>
        </p>

        <div v-if="data.length" id="data">
            <div id="playliststats">
                <h2>Track Averages</h2>
                
                <span>Popularity: {{Math.round(getAverage(this.features.popularity))}}%</span>
            </div>
            
            <ul id="cards-view">
                <Card 
                    v-for="track in data" 
                    :key="track.track.id"
                    :track-data="track"
                />
            </ul>
        </div>
        
    </div>

</template>

<script>

import axios from 'axios';
import Card from './Card.vue';

export default {
  name: 'Playlist',
  components: {
    Card
  },
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
            /* this.features.danceability.push(track.features.danceability);
            this.features.energy.push(track.features.energy);
            this.features.valence.push(track.features.valence);
            this.features.tempo.push(track.features.tempo); */

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
            //console.log(response.status);
            //console.log(response.data);
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
    width: 80%;
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
        max-width: 1200px;
        

    }
}

</style>
