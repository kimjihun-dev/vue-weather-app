<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">

        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">
            <p>{{ weather.weather[0].main }}</p>
            <img :src="'http://openweathermap.org/img/wn/'+ weather.weather[0].icon +'@2x.png'">
          </div>
        </div>

      </div>

    </main>
  </div>
</template>

<script>
// https://youtu.be/JLc-hWsPTUY?t=1347

export default {
  
  name: 'app',
  data(){
    return {
      api_key: '6def828088a884bc59ec60f154433705',
      url_base: 'https://api.openweathermap.org/data/2.5',
      query: '',
      weather: {},
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}/weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["1월", "2월", "3월", "4월", "5월", "6월", "7월", "8월", "9월", "10월", "11월", "12월"];
      let days = ["일요일", "월요일", "화요일", "수요일", "목요일", "금요일", "토요일"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${year}년 ${month} ${date}일 ${day}`;
    }    
  },

}
</script>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { font-family: 'Montserrat', sans-serif; }
#app { background-image: url('./assets/bg-blue.jpg'); background-size: cover; background-position: bottom; transition: 0.4s; }
main { min-height: 100vh; padding: 25px; }
.weather-wrap { margin-top: 150px; }
.search-box { width: 100%; margin-bottom: 30px; }
.search-box .search-bar { display: block; width: 100%; padding: 15px; color: #313131; font-size: 20px; appearance: none; border: none; outline: none; background: none; background-color: rgba(255, 255, 255, 0.5); border-radius: 0 16px 0 16px; transition: all 0.4s; box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25); }
.search-bar:focus { border-radius: 16px 0 16px 0px; background-color: rgba(255, 255, 255, 0.75); }
.location-box { color: #fff; font-size: 32px; font-weight: 900; text-align: center;  }
.date { color: #fff; font-size: 15px; font-weight: 300; text-align: center; }
.weather-box { text-align: center; }
.temp { display: inline-block; color: #fff; font-size: 102px; font-weight: bold; margin: 30px 0px; }
</style>
