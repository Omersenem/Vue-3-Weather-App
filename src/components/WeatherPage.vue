<template>
  <div id="app">
    <div class="main" v-bind:class="{warm: data.state_weather}">
      <div class="search-box">
        <input class="search-bar" type="text" placeholder="Search....." v-model="data.city" @keyup.enter="getApi">
        <div v-if="data.weather" class="">
          <div class="header">
            <h1>{{data.weather.name}}</h1>
            <h3>{{new Date().toLocaleString()}}</h3>
          </div>

          <div class="temp">
            <h2>{{Math.round(data.weather.main.temp)}}&deg;</h2>
          </div>
          <div class="state">
            <h3>{{data.weather.weather[0].main}}
            </h3>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {onMounted, ref} from "vue";
import axios  from "axios";

const data=ref({
  weather:null,
  city:'',
  apiKey:'261514ec6ead072a338a344dce0fb58f',
  current_day:'',
  state_weather: false
});
onMounted(() => {
 getApi()
})
const getApi= async () =>{
  if(data.value.city === ''){
    data.value.city ='Turkey'
  }
  const getWeather= await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?units=metric&q=${data.value.city}&appid=${data.value.apiKey}`
  )
  data.value.city=''
  data.value.weather=getWeather.data
  // eslint-disable-next-line no-empty
  if(Math.round(data.value.weather.main.temp) > 16) {
    data.value.state_weather =true;
  }else {

    data.value.state_weather =false;
  }
  console.log(data.value.weather)

}

</script>



<style scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app{
  background-image: url("@/assets/4.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(18,75,156,0.35), rgba(2,15,22,0.75));
}
.main.warm{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom,rgba(248, 8, 8, 0.35),rgba(246, 6, 6, 0.75));
}
.search-box{
  width: 100%;
  left: 300px;
  margin: 200px;
  position: relative;
}
.search-box .search-bar{
  display: block;
  width: 20%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px;
  background-color: rgba(255,255,255,0.75);
}
.header {
  padding-top: 20px;
  font-size: 20px;
  color: azure;
  box-shadow: rgba(3, 3, 3, 0.3);
}
.temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);

  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.state{
  position: absolute;
  color: #fff;
  font-size: 48px;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  font-style: italic;
  text-align: center;

}

</style>