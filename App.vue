<template>
   <div id="app">
     <main>
       <div class="search-box">
         <input type="text" class="search-bar" placeholder="search..."
          v-model="query"
          @keypress="fetchWeather"
         />
         
       </div>
       <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
         <div class="location-box">
           <div class="location"></div>
           <div class="date">{{dateBuilder()}}</div>
         </div>
       </div>

       <div class="weather-box">
         <div class="temp">{{Math.round(weather.main.temp)}}</div>
         <div class="weather">{{weather.weather[0].main}}</div>
       </div>
     </main>
   </div>
</template>

<script>

export default {
  name: 'App',
  data(){
      retrun {
        api_key : '',
        url_base: 'https://api.openweathermap.org/data/2.5',
        query: '',
        weather: {}
      }
  },
  methods:{
    fetchWeather (e){
      if(e.key === 'Enter'){
        fetch(`${this.url_base}weather?q=${this.query}&&units=metric&&APPID=${this.api_key}`)
        .then(res=>{
          return res.json()
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather=results;
    },
    dateBuilder(){
      let d = new Date();
      let months=['Jan', 'Feb', 'March', 'April', 'May', 'Jun', 'July', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec']
      let days=['Sun', 'Mon', 'Tue', 'Wed', 'Thurs', 'Fri', 'Sat']

      let dat=days[d.getDat()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear()

      return `${day} ${date} ${months} ${year}`
    }
  }
}
</script>

<style scoped>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
}
#app{
  background-image: url('./assets/logo.png');
  background-size: cover;
  background-position: center center;
}
</style>
