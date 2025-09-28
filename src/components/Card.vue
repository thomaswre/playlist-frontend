<template>
    <li class="trackcard">
        <h3>{{ trackData.track.artists[0].name }}</h3> 
        <h4>{{ trackData.track.name }}</h4>
        <img :src="trackData.track.album.images[0].url" class="albumcover" :alt="trackData.track.album.name"/>

        <transition-group tag="div" name="fillWidth" class="featurewrapper" appear>
            <div class="poplabel" key="1">Popularity: {{ trackData.track.popularity }}%</div>
            <div class="feature pop" key="2" :style="{ width: trackData.track.popularity + '%'}"></div>
        </transition-group>
    </li>
</template>

<script setup>
const props = defineProps({
  trackData: {
    type: Object,
    required: true
  }
});
</script>


<style>

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

