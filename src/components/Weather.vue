<template>
  <div>
    <input
      class="border-2 border-black"
      type="text"
      v-model="cityInput"
      name="cityInput"
      placeholder="City Name"
    />
    <button class="btn" @click="getWeather()">Search</button>
    {{ weatherDescription }}
    {{ temp }}
    {{ feelsLike }}
    <img :src="imageUrl" alt="" />
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "WeatherComponent",
  data() {
    return {
      weather: {},
      weatherDescription: "",
      temp: "",
      feelsLike: "",
      cityInput: "",
      imageUrl: "",
      humidity: "",
      windSpeed: "",
    };
  },
  methods: {
    async getWeather() {
      const apiKey = process.env.VUE_APP_WEATHER_API_KEY;
      const query = this.cityInput;
      const unit = "metric";
      const url =
        "https://api.openweathermap.org/data/2.5/weather?q=" +
        query +
        "&appid=" +
        apiKey +
        "&units=" +
        unit +
        "";
      await axios
        .get(url)
        .then((response) => {
          console.log(response);
          // Weather Description
          this.weatherDescription = response.data.weather[0].description;
          // Current Temp
          this.temp = response.data.main.temp;
          // Feels Like Temp
          this.feelsLike = response.data.main.feels_like;
          // Icon
          this.imageUrl =
            "http://openweathermap.org/img/wn/" + response.data.weather[0].icon + "@2x.png";
          //Humidity
          this.humidity = response.data.main.humidity;
          // Wind Speed
          this.windSpeed = response.data.wind.speed;
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>
