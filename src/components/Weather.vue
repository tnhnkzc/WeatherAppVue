<template>
  <div class="flex flex-col items-center mt-8">
    <div
      v-if="weatherDescription"
      class="grid grid-rows-6 md:grid-rows-2 grid-flow-col gap-4 justify-center drop-shadow-lg ease-in duration-800"
    >
      <div
        class="flex justify-center items-center w-52 border-2 rounded-md bg-sky-200 border-sky-200 h-52 drop-shadow-lg hover:scale-125 duration-300 hover:z-10"
      >
        <p>
          {{ weatherDescription }}
        </p>
      </div>
      <transition name="fade">
        <div
          class="flex justify-center w-52 border-2 rounded-md bg-sky-200 border-sky-200 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img :src="imageUrl" alt="" />
        </div>
      </transition>
      <div
        class="flex items-center justify-center w-52 border-2 rounded-md bg-sky-200 border-sky-200 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
      >
        <img class="w-12 mr-4" src="../assets/temperature.png" alt="" />
        <p>
          {{ temp }}
        </p>
      </div>
      <div
        class="flex justify-center items-center w-52 border-2 rounded-md bg-sky-200 border-sky-200 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
      >
        <img class="w-12 mr-4" src="../assets/feelslike.png" alt="" />
        <p>
          {{ feelsLike }}
        </p>
      </div>
      <div
        class="flex items-center justify-center w-52 border-2 rounded-md bg-sky-200 border-sky-200 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
      >
        <img class="w-12 mr-4" src="../assets/humidity.png" alt="" />
        <p>{{ humidity }}</p>
      </div>
      <div
        class="flex justify-center items-center w-52 border-2 rounded-md bg-sky-200 border-sky-200 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
      >
        <img class="w-12 mr-4" src="../assets/wind-speed-icon.jpg" alt="" />
        <p>
          {{ windSpeed }}
        </p>
      </div>
    </div>
    <div class="mt-4 md:mt-4">
      <input
        class="border-2 border-sky-200 rounded placeholder-slate-600"
        type="text"
        v-model="cityInput"
        name="cityInput"
        placeholder="City Name"
      />
      <button class="rounded-full bg-cyan-500 p-2 mt-2 ml-2 md:mt-2" @click="getWeather()">
        Search
      </button>
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
      if (this.cityInput) {
        await axios
          .get(url)
          .then((response) => {
            console.log(response);
            // Weather Description
            this.weatherDescription = "IT IS " + response.data.weather[0].description.toUpperCase();
            // Current Temp
            this.temp = response.data.main.temp + " °C";
            // Feels Like Temp
            this.feelsLike = response.data.main.feels_like + " °C";
            // Icon
            this.imageUrl =
              "http://openweathermap.org/img/wn/" + response.data.weather[0].icon + "@2x.png";
            //Humidity
            this.humidity = response.data.main.humidity + " %";
            // Wind Speed
            this.windSpeed = (response.data.wind.speed * 3.6).toFixed(2) + " KM / H";
          })
          .catch((error) => console.log(error));
      } else {
      }
    },
  },
};
</script>

<style scoped>
.fade-enter-from {
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
}
.fade-enter-active {
  transition: all 2s ease;
}
</style>
