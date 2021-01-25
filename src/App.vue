<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp < 16 ? 
    'warm': '' ">
    <main>
      <div class="search-box">
        <input type="text"
         class="search-bar"
          placeholder="بحث..."
          v-model="query"
          @keypress="fetchWeather"
          />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'" >
        <div class="location-box">
          <div class="location"> {{weather.name}}  {{weather.country}}  </div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp"> {{Math.round(weather.main.temp)}} C</div>
          <div> <img v-bind:src="'http://openweathermap.org/img/wn/' + weather.weather[0].icon +'.png'" /> </div>
          <div class="weather"> {{weather.weather[0].description}} </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      api_key: '500dad23a214684bbd50ce85df349d96',
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{},
}
  },
  created() {
    fetch(`${this.url_base}weather?q=jeddah&units=metric&APPID=${this.api_key}&lang=ar`) 
    .then( res => { 
          return res.json();
        })
    .then(this.setResults);
    },
  methods: {

    fetchWeather(e) {
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}&lang=ar`)
       .then( res => { 
          return res.json();
        })
        .then(this.setResults);
      }
    } , 

    setResults(results){
      this.weather =results;
    } ,
     dateBuilder () {
      let d = new Date();
      let months = ["يناير", "فبراير", "مارس", "ابريل", "مايو", "يونيو", "يوليو", "اغسطس", "سبتمبر", "اكتوبر", "نوفمبر", "ديسمبر"];
      let days = ["الاحد", "الأثنين", "الثلاثاء", "الاربعاء", "الخميس", "الجمعة", "السبت"];
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
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box ;
}
body{
  height: 100vh;
  font-family: 'Tajawal', sans-serif;

}
#app {
  background-image: url('./assets/warm.jpg');
  background-position: center;
  background-size: cover;
  transition: 0.4s;
}
#app.warm {
    background-image: url('./assets/cold1.jpg');
}

main {
  min-height: 100vh;
  padding : 25px;
  background-image: linear-gradient(to bottom ,rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}
.weather-wrap {
  margin-top: 10%;
}
.search-box {
  width:100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  text-align: center;
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size:20px ;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background: none;
  background-color: rgba(255 ,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
    box-shadow: 0px 0px 16px rgba(0,0,0,0.75);
    background-color: rgba(255 ,255,255,0.75);
    border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color:#fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);

}
.location-box .date {
   color:#fff;
  font-size: 17px;
  font-weight: 100;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 24px 25px 0px 25px;
  color : #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px  rgba(0,0,0,0.25);
  background-color: rgba(255 ,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
