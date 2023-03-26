<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ getDate() }}</div>
        </div>
      </div>

      <div class="weather-box" v-if="typeof weather.main != 'undefined'">
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: 'fa33452eb71c1539d42e99b0633205c6',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather () {
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => res.json())
        .then(result => {
          this.weather = result
          console.log(this.weather)
        })
    },
    getDate() {
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let now = new Date()
      let date = days[now.getDay()] + ' ' + now.getDate() + ' ' + months[now.getMonth()] + ' ' + now.getFullYear()
      return date
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{ 
  font-family: 'Poppins', sans-serif;
  background: #f5f5f5;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  height: 50px;
  padding: 0 20px;
  border: none;
  border-radius: 25px;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
  font-size: 18px;
  outline: none;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.location-box .date {
  color: #fff;
  font-size: 18px;
  font-weight: 100;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 90px;
  font-weight: 900;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 25px;
  margin: 30px 0;

  box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}
</style>
