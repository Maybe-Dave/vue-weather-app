<template>
  <main :class="{warm: empty && weather.main.temp >16 }">
    <div class="search-box">
      <input 
        type="text"  
        class="search-bar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
    </div>

    <div class="weather-wrap" v-if="empty" >

      <div class="location-box">
        <div class="location">{{weather.name}}, {{ weather.sys.country }}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}℃</div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from 'vue'

const api_key = ref('3011f0f1406055be1dd3ec07d54a91f5')
const url_base = ref('https://api.openweathermap.org/data/2.5/')
const query = ref('')
const weather = ref({})

function fetchWeather(e){
  if(e.key === 'Enter'){
    fetch(`${url_base.value}weather?q=${query.value}&units=metric&APPID=${api_key.value}`)
    // .then(response => console.log(response))
    .then(res => res.json())
    .then(setResults);
  }
}

function setResults(results){
  weather.value = results
}

function dateBuilder() {
  const d = new Date();
  const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
  const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

  const day = days[d.getDay()];
  const date = d.getDate();
  const month = months[d.getMonth()];
  const year = d.getFullYear();

  return `${day}, ${date} ${month} ${year}`;
}

const empty = computed(()=>{
  return typeof weather.value.main !== 'undefined'
})



</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-position: center;
  background-size: cover;
  transition: 0.4s;
}

#app .warm{
  background-image: url('./assets/warm-bg.jpg');
  background-position: center;
  background-size: cover;
  transition: 0.4s;
}

main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;

}

.search-box .search-bar{
  display: flex;
  width: 100%;
  padding:15px;

  color: #313131;
  font-size:20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;

}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.5);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color:#fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color:#fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}


</style>












<!-- https://api.openweathermap.org/data/2.5/weather?q=London&appid=3011f0f1406055be1dd3ec07d54a91f5 -->
<!-- https://api.openweathermap.org/data/2.5/weather?q=London&appid=3011f0f1406055be1dd3ec07d54a91f5 -->