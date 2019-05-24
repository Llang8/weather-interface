<template>
  <div id="app">
    <div class="top-nav">
      <img @click="onSubmit()" class="search-icon" height=15px src="./assets/search-white.png" alt="search">
      <input type="text" id="search-bar" v-model="query" class="search-bar" placeholder="Enter city or zip code...">
    </div>
    <div class="main-info" v-if="data != null">
      <img src="./assets/location-white.png" height=15px alt="Location icon">
      <h1>{{data.name}}</h1>
      <h2>Fri, May 24 11:11 AM</h2>
      <div class="curr-temp">{{Math.floor(data.main.temp)}}°</div>
      <div class="extra-temp">{{Math.floor(data.main.temp_max)}}° / {{Math.floor(data.main.temp_min)}}°</div>
      <h1 class="forecast">{{data.weather[0].main}}</h1>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import config from '../config.json';

export default {
  name: 'app',
  data () {
    return {
      query: '',
      data: null,
      image: '',
    }
  },
  methods: {
    // Query weather API on search
    onSubmit() {
      console.log(Number(this.query))
      console.log(config);
      if ( !isNaN(this.query) ) {
        axios.get(`http://api.openweathermap.org/data/2.5/weather?zip=${this.query}&APPID=${config.open_weather.api_key}&units=imperial`)
          .then((result) => {
            console.log(result);
            this.data = result.data;
            this.query = '';
            this.image = `http://openweathermap.org/img/w/${this.data.weather[0].icon}.png`;
          });
      } else {
        console.log('test');
        axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${this.query}&APPID=${config.open_weather.api_key}&units=imperial`)
          .then((result) => {
            console.log(result);
            this.data = result.data;
            this.query = '';
            this.image = `http://openweathermap.org/img/w/${this.data.weather[0].icon}.png`;
          });        
        /* axios.get(`http://api.openweathermap.org/data/2.5/forecast?q=${this.query}&appid=e325c95def146ec0f6463c1ba75ad893`)
          .then((result) => {
            console.log(result);
          }); */
      }
    }
  },
  created() {
    axios.get(`http://api.ipstack.com/check?access_key=${config.ipstack.api_key}`).then((result) => {
      this.query = result.data.zip;
      this.onSubmit();
    });
  }
}

</script>

<style>
/* Import Roboto Font */
@import url('https://fonts.googleapis.com/css?family=Roboto&display=swap');

body {
  overflow: hidden;
  background: linear-gradient(140deg, rgb(255, 147, 6) 10%, rgb(241, 20, 223) 100%);
  height: 100vh;
  width: 100vw;
  font-family: Roboto;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
#app {
  height: 740px;
  width: 360px;
  background: linear-gradient(180deg, rgb(35, 33, 66) 10%, rgb(2, 168, 154) 100%);
  border-radius: 25px;
  border: 5px solid black;
  border-top: 40px solid black;
  overflow-x: hidden;
  overflow-y: auto;
}

/* BEGIN CSS FOR TOP NAV */
.top-nav {
  position: relative;
  width: 100%;
  height: 20px;
  display: flex;
}
.search-icon {
  position: absolute;
  right: 10px;
  top: 10px;
  cursor: pointer;
}
.search-bar {
  position: absolute;
  font-size: 10px;
  padding: 3px 10px;
  width: 1px;
  border: 0px;
  right: 40px;
  top: -20px;
  border-radius: 10px;
  animation: 2s searchIn forwards;
}

@keyframes searchIn {
  0% {
    width: 1px;
    top: -20px;
  }
  25% {
    width: 1px;
    top: 10px;
  }
  100% {
    width: 150px;
    top: 10px;
  }
}
/* END CSS FOR TOP NAV */
/* BEGIN CSS FOR FIRST BOX */
.main-info {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
  margin-top: 40px;
}

.main-info > h1 {
  font-size: 20px;
  margin: 0;
}
.main-info > h2 {
  font-size: 15px;
  color: rgb(200,200,200);
  margin: 5px 0px 30px 0px;
  font-weight: lighter;
}

.curr-temp {
  font-size: 65px;
  font-weight:thin;
}

.extra-temp {
  font-size: 15px;
  font-weight: lighter;
}

.forecast  {
  margin: 25px 0px !important;
}

/* Handle shifting to full screen view after certain size */
@media only screen and (max-width: 768px) {
  body {
    margin: 0;
  }
  #app {
    width: 100vw;
    height: 100vh;
    border: 0px;
    border-radius: 0;
  }
}
</style>
