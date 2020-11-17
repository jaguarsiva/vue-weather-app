<template>
<div id="app" :class="isCold ? 'cold' : 'warm'">

  <div class="main-box">
    <form @submit.prevent="callAPI" class="search-box">
      <input type="text" class="search-bar" 
        v-model="city" 
        placeholder="Search for City.."
      />
      <button type="submit" class="search-btn">
        <img src="./assets/search.png" alt="search">
      </button>
    </form>

    <h1 class="location" v-if="details"> {{ details.name }}, {{ details.country }} </h1>
    <p class="date"> {{ getDate() }}</p>
    <h2 class="temp" v-if="details">{{ details.temp }}°c</h2>
    <div class="weather" v-if="details">{{ details.desc}}</div>
  </div>

</div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiKey: "f2067413b9585acff49c6598191d03ed",
      baseURL: "https://api.openweathermap.org/data/2.5/weather",
      city: "",
      details: null,
      isCold: false
    }
  },
  methods: {
    callAPI() {
      axios.get(`${this.baseURL}?q=${this.city}&appid=${this.apiKey}&units=metric`)
        .then( ({data}) => {
          this.details = {};
          this.details.name = data.name;
          this.details.country = data.sys.country;
          this.details.temp = data.main.temp;
          this.details.desc = data.weather[0].description;
          if(this.details.temp < 25) this.isCold = true;
          else this.isCold = false;
        })
        .catch( error => {
          console.log(error.message);
        });
    },
    getDate(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>

/* General Stylings */

*
{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat",sans-serif;
}

html { height: 100%; overflow: hidden; }

body { height: 100%; overflow: hidden; }

@font-face
{
  src: url("./assets/Montserrat-Regular.ttf");
  font-family: "Montserrat";
  font-display: block;
}

/* Custom Stylings */

#app
{
  height: 100%;
  background-size: cover;
}

.warm { background-image: url("./assets/warm.jpg"); }

.cold { background-image: url("./assets/cold.jpg"); }

.main-box
{
  width: 100%;
  max-width: 768px;
  height: 100%;
  padding: 50px 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 0 auto;
  text-align: center;
}

.search-box
{
  width: 100%;
  display: flex;
  position: relative;
}

.search-bar 
{
  width: 100%;
  padding: 15px;
  font-size: 20px;
  border: none; outline: none;
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  color: #222222;
  font-weight: bold;
  transition: all 0.2s ease;
  margin-bottom: 30px;
}

.search-bar::placeholder { font-weight: normal; }

.search-bar::selection { background-color: #000000; color: #ffffff; }

.search-bar:focus 
{
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.2);
  border-radius: 16px 0px 16px 0px;
}

.search-btn
{
  background: none;
  border: none;
  outline: none;
  position: absolute;
  right: 12px; top: 12px;
  cursor: pointer;
}

.search-btn img { height: 30px; }

.search-btn:hover img ,.search-btn:focus img {  opacity: 0.65; }

.location
{
  text-transform: uppercase;
  font-size: 45px;
  font-weight: bold;
  text-align: center;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.5);
  margin: 33px 0 12px;
}

.date
{
  font-size: 22px;
  font-weight: bold;
  font-style: italic;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.5);
  line-height: 1.5;
}

.temp
{
  display: inline-block;
  padding: 10px 25px;
  font-size: 102px;
  font-weight: 900;
  border-radius: 16px;
  margin: 50px auto 30px;
}

.weather
{
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  text-transform: capitalize;
  line-height: 1.25;
}

.warm { color: #ffffff; }

.warm .temp 
{ 
  background-color:rgba(255, 255, 255, 0.25);
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.cold { color: #000000; }

.cold * { text-shadow: 3px 3px rgba(255, 255, 255, 0.25); }

.cold .temp 
{ 
  background-color:rgba(0, 0, 0, 0.25); 
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  box-shadow: 3px 6px 12px rgba(255, 255, 255, 0.25);
}

@media (max-width: 768px)
{
  .temp { font-size: 60px; }
}

</style>