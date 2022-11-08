<template>
  <div class="flex flex-col items-center mt-8">
    <div class="grid grid-rows-2 grid-flow-col gap-4 justify-center">
      <div class="flex items-center">
        <p>
          {{ weatherDescription }}
        </p>
      </div>
      <div class="flex items-center">
        <p>
          {{ temp }}
        </p>
      </div>
      <div class="flex items-center">
        <p>
          {{ feelsLike }}
        </p>
      </div>
      <div class="flex items-center">
        <p>{{ humidity }}</p>
      </div>
      <div class="flex items-center">
        <p>
          {{ windSpeed }}
        </p>
      </div>
      <div class="flex justify-center">
        <img :src="imageUrl" alt="" />
      </div>
    </div>
    <div>
      <input
        class="border-2 border-black rounded"
        type="text"
        v-model="cityInput"
        name="cityInput"
        placeholder="City Name"
      />
      <button class="rounded-full bg-cyan-500 p-2 ml-2" @click="getWeather()">Search</button>
    </div>
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
          this.weatherDescription = "IT IS " + response.data.weather[0].description.toUpperCase();
          // Current Temp
          this.temp = "Temperature : " + response.data.main.temp + " °C";
          // Feels Like Temp
          this.feelsLike = "Real Feel : " + response.data.main.feels_like + " °C";
          // Icon
          this.imageUrl =
            "http://openweathermap.org/img/wn/" + response.data.weather[0].icon + "@2x.png";
          //Humidity
          this.humidity = "Humidity : " + response.data.main.humidity + " %";
          // Wind Speed
          this.windSpeed =
            "Wind Speed : " + (response.data.wind.speed * 3.6).toFixed(2) + " KM / H";
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>
