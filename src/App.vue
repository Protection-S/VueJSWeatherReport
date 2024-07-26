<template>
  <div class="wrapper">
    <h1>Узнайте погоду в вашем городе</h1>
    <p>Узнать в {{ city == "" ? "в вашем городе" : cityName }}</p>
    <div class="input-container">
      <input type="text" placeholder="Введите название города" v-model="city">
      <button v-show="city != ''" @click="getWeather()">Узнать</button>
      <p class="error">{{ error }}</p>
    </div>
    <div v-if="weather" class="weather-info">
      <p>Температура: {{ weather.main.temp }}Градусов</p>
      <p>Ощущается как: {{ weather.main.feels_like }}Градусов</p>
      <p>Влажность: {{ weather.main.humidity }}%</p>
      <p>Погодные условия: {{ weather.weather[0].description }}</p>
    </div>
  </div>
</template>

<style scoped>
.error{
  color:red
}
  .wrapper {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    border-radius: 10px;
    background-color: #f0f0f0;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  h1 {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
  }

  .input-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  input[type="text"] {
    width: 70%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    margin-right: 10px;
  }

  button {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-color: #1b911f;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #45a049;
  }
</style>

<script>
import { computed } from 'vue';
import axios from 'axios'
export default {
  data() {
    return {
      city: "",
      error: "",
      weather: ""
    };
  },
  computed: {
    cityName() {
      return `'${this.city}'`;
    }
  },
  methods: {
    async getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Непруха";
        return false;
      } else {
        this.error = "";
      }

      try {
        const response = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b9c6e03bd92ff2d8a0af1f787056a955`
        );
        this.weather = response.data;
      } catch (error) {
        this.error = error.message;
      }
    }

  }
};
</script>