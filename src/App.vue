<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search" 
          v-model="query"
          @keypress="getWeather"
        />

      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-container">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ showdate() }}</div>
        </div>

        <div class="weather-container">
          <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{ weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data () {
    return {
      api_key: 'b91ffe2712852827072c4e8444d6c133',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    getWeather(e) {
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(result) {
      this.weather = result;
    },
    showdate() {
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;

}

.rain {
  height: 100vh;
  background: url('https://img.pngio.com/rain-hd-png-transparent-rain-hdpng-images-pluspng-raining-hd-png-1920_1080.png');
  animation: rain .4s linear infinite;
}

@keyframes rain {
  0%{
    background-position: 0% 0%;
  }
  100%{
    background-position: 10% 100%;
  }
}

#app{
  background-image: url('./assets/cold.jpg');
  background-size: cover;
  transition: .4s;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

main{
  min-height: 100vh;
  padding: 25px;
  padding-top: 80px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  
  appearance: none;
  outline: none;
  border: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.5);
  border-radius: 15px 0px 15px 0px;
  transition: .5s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.5);
  border-radius: 0px 15px 0px 15px;
}

.location-container .location{
  color: #fff;
  font-size: 28px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,.25);
}

.location-container .date{
  color: #fff;
  font-size: 20px;
  font-weight: 200;
  text-align: center;
  font-style: italic;
  padding: 10px;
}

.weather-container {
  text-align: center;

}

.weather-container .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  margin: 30px 0px;
  border-radius: 15px;

  text-shadow: 3px 6px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.2);

  box-shadow: 3px 6px rgba(0,0,0,.2);
}

.weather-container .weather{
  color: #fff;
  font-size: 35px;
  font-weight: 500;
  text-shadow: 3px 6px rgba(0,0,0,.25);
}
</style>
