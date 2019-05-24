<template>
  <div id="app">
    <img src="./assets/background.jpg" alt="" class="bg-img">
    <div class="bg-hue"></div>
    <div id="search-container">
      <h1>Get Current Weather</h1>
      <input type="text" id="search-box" v-model="query" placeholder="Enter city name or zipcode...">
      <button @click="onSubmit()">Search</button>
      <div id="result" v-if="data != null">
        <h1>{{data.name}}</h1>
        <img height="100px" v-if="image != ''" :src="image" alt="">
        <p>{{data.weather[0].main}}</p>
        <p>Current Temperature: {{data.main.temp}}</p>
        <p>Min Temperature: {{data.main.temp_min}}</p>
        <p>Max Temperature: {{data.main.temp_max}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'app',
  data () {
    return {
      query: '',
      data: null,
      image: ''
    }
  },
  methods: {
    // Query weather API on search
    onSubmit() {
      console.log(Number(this.query))
      if ( !isNaN(this.query) ) {
        axios.get(`http://api.openweathermap.org/data/2.5/weather?zip=${this.query}&APPID=e325c95def146ec0f6463c1ba75ad893&units=imperial`)
          .then((result) => {
            console.log(result);
            this.data = result.data;
            this.image = `http://openweathermap.org/img/w/${this.data.weather[0].icon}.png`;
          });
      } else {
        console.log('test');
        axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${this.query}&APPID=e325c95def146ec0f6463c1ba75ad893&units=imperial`)
          .then((result) => {
            console.log(result);
            this.data = result.data;
            this.image = `http://openweathermap.org/img/w/${this.data.weather[0].icon}.png`;
          });        
      }
    },
  }
}
</script>

<style>
body {
  overflow: hidden;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

#search-container {
  background: white;
  min-width: 400px;
  padding: 25px;
  border-radius: 10px;
}

#result {
  margin-top: 25px;
}

h1 {
  margin: 0px 0px 10px 0px;
  font-size: 25px;
}

input {
  border-radius: 5px;
  padding: 5px;
  width: 70%;
  max-width: 300px;
  border: 1px solid black;
}

button {
  padding: 5px;
  margin-left: 5px;
  border-radius: 5px;
  border: 1px solid black;
  background: white;
}
button:hover {
  background: rgb(200,200,200);
  cursor: pointer;
}

.search-bar {
  margin-bottom: 20px;
}
.bg-img {
  z-index: -1000;
  /* Set rules to fill background */
  min-height: 100%;
  min-width: 1024px;
	
  /* Set up proportionate scaling */
  width: 100%;
  height: auto;
	
  /* Set up positioning */
  position: fixed;
  top: 0;
  left: 0;
}

.bg-hue {
  z-index: -999;
  /* Set rules to fill background */
  min-height: 100%;
  min-width: 1024px;
	
  /* Set up proportionate scaling */
  width: 100%;
  height: auto;
	
  /* Set up positioning */
  position: fixed;
  top: 0;
  left: 0;

  background-color:rgba(94, 72, 107, 0.8);
}
</style>
