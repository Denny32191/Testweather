<template>
<div class="wrapper">
  <h1>Погода</h1>
  <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
  <input type="text" v-model="city" placeholder="Введите город"/><!--v-model обработчик событий inputa -->
  <button v-if="city != ''" v-on:click="getWeather()">Получить погоду</button>
  <button disabled v-else>введите название города</button><!--v-show для скрывания кнопки если ничего не введенно-->
   <!--v if условие если ничего не введено будет отображаться вторая кнопка (введите название) -->
   <p class="error">{{  error }}</p>
<div v-if="info != null">
  <p >{{ showTemp}}</p>
  <p>{{ showFeelsLike }}</p>
  <p>{{ showMinTemp }}</p>
  <p>{{ showMaxTemp }}</p>
</div>
   
</div>
</template>

<script>

import axios from 'axios'
export default {
data() {
  return {
    city: "",
    error: "",
    info: null
  }
},
computed: {
  cityName() {
    return "" + this.city + ""
  },
  showTemp() {
    return "Температура: " + this.info.main.temp
  },
  showFeelsLike() {
    return "Ощущается как:" + this.info.main.feels_like
  },
  showMinTemp() {
    return "Минимальная температура: " + this.info.main.temp_min
  },
  showMaxTemp() {
    return "Максимальная температура: " + this.info.main.temp_max
  },
},
methods: {
  getWeather() {
    if(this.city.trim().length < 2) {
      this.error = 'Напишите город'
      return false
    }
    this.error = ''

    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=f9489b4a1f411372f9ff133af825c967`)
    .then(response => {
        this.info = response.data  ;
    })
    .catch(error => {
        this.error = 'Ошибка при получении погоды';
        console.error(error);
    });
  }
}
}
</script>

<style scoped>
.error {
  color: red;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgb(19,56,62);
  padding: 20px;
background: linear-gradient(332deg, rgba(19,56,62,1) 0%, rgba(67,184,208,1) 100%);
text-align: center;
color: white;
}
.wrapper h1 {
margin-top: 50px;


}
.wrapper p {
  margin-top: 20px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid  rgb(113, 164, 173);;
  color: white;
  font-size: 14px;
  padding:5px ;
  outline: none;
}
.wrapper input:focus {
  border-bottom-color: 2px solid  rgb(113, 164, 173);
}
.wrapper button:disabled {
  background-color: aqua;
}
.wrapper button {
  background: rgb(86, 180, 204)(0, 140, 255);
  color:rgb(86, 180, 204);
  border-radius: 10px;
  border: 2px solid rgb(0, 217, 255);
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

</style>
