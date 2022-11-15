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
      cityName: "",
      tempMax: "",
      tempMin: "",
      bgImage: "",
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
            this.temp = Math.round(response.data.main.temp) + " °C";
            // Feels Like Temp
            this.feelsLike = Math.round(response.data.main.feels_like) + " °C";
            // Icon
            this.imageUrl =
              "http://openweathermap.org/img/wn/" + response.data.weather[0].icon + "@2x.png";
            // Humidity
            this.humidity = response.data.main.humidity + " %";
            // Wind Speed
            this.windSpeed = Math.round((response.data.wind.speed * 3.6).toFixed(2)) + " KM / H";
            // City Name
            this.cityName = response.data.name;
            // Max and Min Temp
            this.tempMax = Math.round(response.data.main.temp_max) + " °C";
            this.tempMin = Math.round(response.data.main.temp_min) + " °C";
            if (response.data.weather[0].main === "Rain") {
              this.bgImage = "'../assets/rain.jpg'";
            }
          })
          .catch((error) => console.log(error));
      } else {
      }
    },
  },
};
</script>

<template>
  <div class="flex items-center flex-col">
    <div
      id="mainContainer"
      :style="`backgroundImage: url(${bgImage})`"
      class="flex flex-col items-center mt-8 md:w-1/2 min-h-min"
    >
      <div
        v-if="weatherDescription"
        class="mt-2 mb-2 mr-2 ml-2 grid grid-cols-2 md:grid-rows-3 md:grid-flow-col gap-4 justify-center drop-shadow-lg ease-in duration-800"
      >
        <div
          class="flex justify-center items-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg hover:scale-125 duration-300 hover:z-10"
        >
          <p class="text-white text-xs md:text-base text-xs md:text-base">
            {{ cityName }}
          </p>
        </div>
        <div
          class="flex justify-center items-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg hover:scale-125 duration-300 hover:z-10"
        >
          <p class="text-white text-xs md:text-base">
            {{ weatherDescription }}
          </p>
        </div>
        <transition name="fade">
          <div
            class="flex justify-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
          >
            <img class="w-full" :src="imageUrl" alt="" />
          </div>
        </transition>
        <div
          class="flex items-center justify-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/temperature.png" alt="" />
          <p class="text-white text-xs md:text-base">
            {{ temp }}
          </p>
        </div>
        <div
          class="flex justify-center items-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/feelslike.png" alt="" />
          <p class="text-white text-xs md:text-base">
            {{ feelsLike }}
          </p>
        </div>
        <div
          class="flex items-center justify-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/humidity.png" alt="" />
          <p class="text-white text-xs md:text-base">{{ humidity }}</p>
        </div>
        <div
          class="flex items-center justify-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/tempMax.png" alt="" />
          <p class="text-white text-xs md:text-base">{{ tempMax }}</p>
        </div>
        <div
          class="flex items-center justify-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/tempMin.png" alt="" />
          <p class="text-white text-xs md:text-base">{{ tempMin }}</p>
        </div>
        <div
          class="flex justify-center items-center w-36 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/wind-speed-icon.jpg" alt="" />
          <p class="text-white text-xs md:text-base">
            {{ windSpeed }}
          </p>
        </div>
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
#mainContainer {
  background-image: url("../assets/temperature.png");
  background-size: 100%;
  background-repeat: no-repeat;
  border-radius: 10px;
}
/* Media Query */
@media screen and (max-width: 600px) {
  #mainContainer {
    /* background-image: url(C:\Users\GEN-06\Desktop\Home\weatherApp\WeatherAppVue\src\assets\foggy.jpg); */
    background-repeat: no-repeat;
    background-size: cover;
    border-radius: 10px;
  }
}
</style>
