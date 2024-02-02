<script>
import axios from 'axios';
export default {
  data(){
    return{
      city: "",
      error: "",
      info: null
    }
  },
  computed:{
    cityName(){
      return "«" + this.city + "»"
    },
    showTemp(){
      return "Temperature " + this.info.temp + "℃"
    },
    showFeelsLike(){
      return "Feels like " + this.info.feels_like
    },
    showMinTemp(){
      return "Min temperature " + this.info.temp_min + "℃"
    },
    showMaxTemp(){
      return "Max temperature " + this.info.temp_max + "℃"
    }
  },
  methods:{
    getWeather(){
      if(this.city.trim().length<2){
        this.error = "Название города должно содержать больше 1 символа"
        return false
      }
      this.error = "";
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=4166e96b493ff4aa6e54fa747352bb13`)
      .then(res=> this.info = res.data.main)
    }
  }
}</script>

<template>
<div className="wrapper">
  <h1>Weather App </h1>
  <p>What's the weather like today in {{ city ==""? "your city": cityName }}?</p>
  <input type="text" @input="this.error = ''" v-model="city" placeholder="Enter city name">
  <button v-if="city!=''" @click="getWeather()">Get weather</button>
  <button disabled v-else>Enter city name</button>
  <p class="error">{{ error }}</p>

  <div v-if="info!=null">
    <p>{{ showTemp }}</p>
    <p>{{ showFeelsLike }}</p>
    <p>{{ showMinTemp }}</p>
    <p>{{ showMaxTemp }}</p>

  </div>
</div>
</template>

<style scoped>
.wrapper{
  text-align: center;
  align-items: center;
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: black;
  color: white;
}
.wrapper h1{
margin-top: 50px;
}
.wrapper p{
  margin-top: 20px;
}
.wrapper input{
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}
.wrapper input:focus{
border-bottom-color: #f8d8ce;
}
.wrapper button{
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:disabled{
  cursor: not-allowed;
  background-color: #645e4b;
}
.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}
.error{
  color: rgb(133, 24, 24);
}
</style>
