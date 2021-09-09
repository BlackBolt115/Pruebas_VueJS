<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ?
    'warm' : ''">
    <main>
      <div class="form">
        <form>
          <input type="text" v-model="text"/>
        </form>
      </div>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Buscar..." v-model='query' @keypress="fetchWeather"/>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
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
      api_key: '95d03a8baf95e98f114d0a2bf7c05b55',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {},
      url_nodeRed: 'http://192.168.100.242:1880/test'
    }
  },
  methods: {
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
      let days = ["Sunday","Monday","Tuesday","Wednesday","Thrusday","Friday","Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;

    },

    fetchNodeRed(e){
      if(e.key == "Enter"){
        fetch(`${this.url_nodeRed}`)
        //.then(res => {
        //  return res.json();
        //}).then(this.setResults);
      }
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
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
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
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.75);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #FFFFFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 2px rgba(0,0,0,0.25);
}

.location-box .date{
  color: #FFFFFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #FFFFFF;
  font-size: 100px;
  font-weight: 900px;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: #FFFFFF;
  font-size: 40px;
  font-weight: 700px;
  font-style: italic;
  text-shadow: 2px 4px rgba(0,0,0,0.25);
}

</style>
