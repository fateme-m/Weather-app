<template>
 <div id="app" :class="(typeof weather2.temp_c != 'undefined' && weather2.temp_c > 22 ? 'warm' : '')">
  <main>
    <div class="search-box">
      <input 
        type="text" 
        class="search-bar"
        placeholder="search..."
        v-model="query" 
        @keypress="fetchWeather">
    </div>
    <div class="weather-wrap" >
      <div class="location-box">
        <div class="location">{{weather.name }} {{weather.country }}</div>
        <div class="date">{{ weather.localtime }}</div>
      </div>
      <div class="weather-box">
        <div class="temp" v-show="weather2.temp_c">{{ Math.round( weather2.temp_c) }}°c</div>
        <div class="weather">{{ Condition[0] }}</div>
      </div>
    </div>
  </main>
 </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      apiKey: '748feb673d1445bd8be174944230408',
      query :'',
      weather :{},
      weather2:{},
      Condition:''
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == 'Enter'){
        fetch(`http://api.weatherapi.com/v1/current.json?key=${this.apiKey}&q=${this.query}&aqi=yes`)
        .then(res =>{
          return res.json()
        }).then(this.setResults)
      }
    },
    setResults(result){
        this.weather = result.location
        this.weather2 = result.current
        this.Condition = Object.values(this.weather2.condition);
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
  font-family: sans-serif;
}

#app{
  background-image: url("./assets/cold.jpg");
  background-size: cover ;
  background-position:bottom ;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/warm.jpg');
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0.0.0.0.25) , rgba(0.0.0.0.75));

}
.search-box{
  width: 100%;
  margin-bottom: 30px;
  justify-content: center;
  display: flex;
}
.search-box .search-bar{
  display: block;
  width: 80%;
  padding: 15px;
  color: #313131;
  font-size:20px ;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255,255,255,0.5);
  box-shadow: 0 0 17px rgba(0, 0, 0, 0.30);
  border-radius:15px ;
  transition: 0.4s;
}
.search-bar:focus{
  background-color: rgba(255, 255, 255, 0.75);
}
.location-box .date {
  font-size: large;
  font-style: italic;
  color: rgba(255, 255, 255, 0.6);
}
.date, .location{
  font-size: 32px;
}

.location{
  font-weight: 500;

}
.weather-wrap{
  text-align: center;
  color: #fff;
  text-shadow:1px 3px rgba(0, 0, 0, 0.25) ;

}
.weather-wrap .temp{
  display: inline-block;
  padding: 10px 25px;
  font-size: 12vh;
  background-color: rgba(255, 255, 255 , 0.25);
  border-radius: 15px;
  margin: 30px 0;
  box-shadow: 0 0 17px rgba(0, 0, 0, 0.30);
}
.weather-box .weather{
  font-size: 40px;
}
</style>
