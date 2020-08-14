<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp < 16 ? 'cold' : ''">
    <div class="main-box">
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" v-on:keyup.enter="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ getDate() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    async fetchWeather() {
      const res = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`);
      this.weather = await res.json();
    },
    getDate() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date}th ${month}, ${year}`
    }
  }
}
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
  background-image: url('./assets/warm-bg.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transition: 0.4s;
}
#app.cold {
  background-image: url('./assets/cold-bg.jpg');
}

.main-box{
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: start;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box{
  width: 100%;
  margin-top: 4rem;
  margin-bottom: 2rem;
  padding: 15px;
}

.search-bar{
  width: 100%;
  border-radius: 0 16px 0 16px;
  padding: 15px;

  color: #313131;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: #d6cdcd;
  transition: 0.4s;
  font-size: 1rem;
}
.search-box .search-bar:focus{
  background-color: #f0eeeee7;
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.75);
  border-radius: 16px 0 16px 0;
}

.weather-wrap{
  margin-top: 3rem;
}

.location-box .location{
  color: #fff;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #fff;
  font-size: 1.4rem;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box{
  text-align: center;
}
.weather-box .temperature{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 5.2rem;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: #d6d4d479;
  border-radius: 1rem;
  margin: 1.9rem 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color: #fff;
  font-weight: 700;
  font-style: italic;
  font-size: 3rem;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
