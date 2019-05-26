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
    <div class="divider-container" v-if="data != null"><div class="divider"></div></div> 
    <div class="hourly" v-if="data != null">
      <h1>Tomorrow's Forecast</h1>
      <div class="hourly-container">
        <div class="hourly-object" v-for='(data, index) in hourlyData'>
          <h2>{{data.dt_txt.split(' ')[1].split(':')[0]}}</h2>
          <h2>{{Math.floor(data.main.temp)}}°</h2>
          <h2>{{data.weather[0].main}}</h2>
        </div>
      </div>
    </div>
    <div class="divider-container" v-if="data != null"><div class="divider"></div></div>
    <div class="details" v-if="data != null">
      <h1>Details</h1>
      <div class="detail">
        <div class="key">Wind Degrees</div>
        <div class="value">{{data.wind.deg}}</div>
      </div>
      <div class="detail">
        <div class="key">Wind Speed</div>
        <div class="value">{{data.wind.speed}}</div>
      </div>
      <div class="detail">
        <div class="key">Humidity</div>
        <div class="value">{{data.main.humidity}}%</div>
      </div>
      <div class="detail">
        <div class="key">Pressure</div>
        <div class="value">{{data.main.pressure}}</div>
      </div>
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
      hourlyData: null,
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
        axios.get(`http://api.openweathermap.org/data/2.5/forecast?zip=${this.query}&appid=e325c95def146ec0f6463c1ba75ad893&units=imperial`)
          .then((result) => {
            console.log(result);
            this.hourlyData = result.data.list.slice(0,8);
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
        axios.get(`http://api.openweathermap.org/data/2.5/forecast?q=${this.query}&appid=e325c95def146ec0f6463c1ba75ad893&units=imperial`)
          .then((result) => {
            console.log(result);
            this.hourlyData = result.data.list.slice(0,8);
          });
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
  background: linear-gradient(180deg, rgb(146, 49, 150) 10%, rgb(2, 168, 154) 100%);
  border-radius: 25px;
  border: 5px solid black;
  border-top: 40px solid black;
  overflow-x: hidden;
  overflow-y: auto;
}
.divider-container {
  width: 100%;
  display: flex;
  margin: 25px 0px;
  align-items: center;
  justify-content: center;
}
.divider {
  width: 90%;
  border-bottom: 1px solid white;
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

h1 {
  font-size: 20px;
  margin: 0;
}
h2 {
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
/* END CSS FOR MAIN INFO BOX */
/* BEGIN CSS FOR HOURLY REPORT */
.hourly {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.hourly-container {
  display: flex;
  justify-content: space-around;
  width: 90%;
  margin-top: 10px;
}
.hourly-object {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-right: 1px solid white;
  border-left: 1px solid white;
}
.hourly-object > h2 {
  margin: 5px;
}
/* END CSS FOR HOURLY REPORT */
/* BEGIN CSS FOR DETAILS */
.details {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.detail {
  margin: 10px 0px;
  width: 80%;
  padding-bottom: 15px;
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid white;
}
/* END CSS FOR DETAILS */
/* BEGIN CSS FOR SCROLLBAR */
/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: rgba(0,0,0,0); 
}
 
/* Handle */
::-webkit-scrollbar-thumb {
  background: rgba(173, 173, 173,.5); 
  border-radius: 10px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: rgba(173, 173, 173,.75); 
}
/* END CSS FOR SCROLLBAR */
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
