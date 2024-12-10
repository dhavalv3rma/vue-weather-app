<script>

export default {
  name: 'App',
  // components: 'helloworld',
  data() {
    return {
      api_key: 'f9f8db187a3dbf915349ad4d418167bb',
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },

  methods: {
    fetchweather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },

    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month}, ${year}`;
    }
  }
}
</script>


<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 17 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchweather">
      </div>

      <!-- <div class="weather-wrap" v-if="typeof weather.main != 'undefined' && weather.main.cod == '404'">NOT FOUND</div> -->

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
        <div class="temp-data text-center">
          <!-- <span>Feels like: {{ Math.round(weather.main.feels_like) }}°c</span> -->
          <span>Max: {{ weather.main.temp_max }}°c</span><br>
          <span>Min: {{ weather.main.temp_min }}°c</span>
          <br><br>
          <span>Visibility: {{ Math.round(weather.visibility) }} </span>
        </div>
      </div>
    </main>
  </div>
  <HelloWorld />
</template>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#app {
  background: #5c838f;
  transition: 0.4s;
  color: #ffffff;
}

#app.warm {
  background: #b1ad75;
}

main {
  height: 100vh;
  padding: 25px;
}
.text-center{
  text-align: center;
}
.temp-data{
  margin-top:20px;
}

.search-box .search-bar {
  width: 100%;
  padding: 10px 5px;
  margin-bottom: 40px;
  font-size: 32px;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px;
}

.location-box .location {
  color: #ffffff;
  text-align: center;
  font-size: 32px;
  font-weight: 500;
  /* text-shadow: 1px 3px #000000 */
}

.location-box .date {
  color: #ffffff;
  text-align: center;
  font-size: 18px;
  margin-top: 10px
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #ffffff;
  font-size: 66px;
  margin-top: 10px;
  /* text-shadow: 1px 3px #000000; */
  background-color: #2d2d2d70;
}
</style>
