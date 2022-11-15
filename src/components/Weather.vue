<script>
import axios from "axios";
export default {
  name: "WeatherComponent",
  data() {
    return {
      weather: {},
      weatherDescription: "",
      mainDescription: "",
      temp: "",
      feelsLike: "",
      cityInput: "",
      imageUrl: "",
      humidity: "",
      windSpeed: "",
      cityName: "",
      country: "",
      tempMax: "",
      tempMin: "",
      bgImage: "../assets/rain.jpg",
    };
  },
  methods: {
    dateBuilder() {
      let d = new Date();
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year} `;
    },
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
            this.mainDescription = response.data.weather[0].main;
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
            // Country Name
            this.country = response.data.sys.country;
            // Max and Min Temp
            this.tempMax = Math.round(response.data.main.temp_max) + " °C";
            this.tempMin = Math.round(response.data.main.temp_min) + " °C";
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
    <div class="mt-4 md:mt-4">
      <p class="text-black font-bold text-lg md:text-2xl mb-8">
        {{ dateBuilder() }}
      </p>
      <input
        class="block w-full rounded-md border-gray-300 pl-2 pr-12 focus:border-indigo-500 focus:ring-indigo-500 sm:text-md"
        type="text"
        v-model="cityInput"
        name="cityInput"
        placeholder="City Name"
      />
      <button
        class="rounded-md hover:rounded-full bg-indigo-500 hover:bg-indigo-600 p-2 mt-8 ml-2 md:mt-8"
        @click="getWeather()"
      >
        Search
      </button>
    </div>
    <div
      id="mainContainer"
      class="flex flex-col items-center mt-8 md:w-1/2 min-h-min"
      :class="
        mainDescription == 'Rain' || mainDescription == 'Drizzle' || mainDescription == 'Storm'
          ? 'rainy'
          : 'sunny'
      "
      v-if="weatherDescription"
    >
      <div
        class="mt-2 mb-2 mr-2 ml-2 grid grid-cols-3 md:grid-rows-3 md:grid-flow-col gap-4 justify-center drop-shadow-lg ease-in duration-800"
      >
        <div
          class="flex justify-center items-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg hover:scale-125 duration-300 hover:z-10"
        >
          <p class="text-black font-bold text-lg md:text-2xl font-bold">
            {{ cityName }}, {{ country }}
          </p>
        </div>

        <div
          class="flex justify-center items-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg hover:scale-125 duration-300 hover:z-10"
        >
          <p class="text-black font-bold text-lg md:text-2xl">
            {{ weatherDescription }}
          </p>
        </div>
        <transition name="fade">
          <div
            class="flex justify-center items-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
          >
            <img class="w-32 h-32 md:w-full md:h-full" :src="imageUrl" alt="" />
          </div>
        </transition>
        <div
          class="flex items-center justify-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/temperature.png" alt="" />
          <p class="text-black font-bold text-lg md:text-2xl">
            {{ temp }}
          </p>
        </div>
        <div
          class="flex justify-center items-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/feelslike.png" alt="" />
          <p class="text-black font-bold text-lg md:text-2xl">
            {{ feelsLike }}
          </p>
        </div>
        <div
          class="flex items-center justify-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/humidity.png" alt="" />
          <p class="text-black font-bold text-lg md:text-2xl">{{ humidity }}</p>
        </div>
        <div
          class="flex items-center justify-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/tempMax.png" alt="" />
          <p class="text-black font-bold text-lg md:text-2xl">{{ tempMax }}</p>
        </div>
        <div
          class="flex items-center justify-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/tempMin.png" alt="" />
          <p class="text-black font-bold text-lg md:text-2xl">{{ tempMin }}</p>
        </div>
        <div
          class="flex justify-center items-center w-28 h-28 md:h-48 md:w-48 border-1 rounded-md bg-slate-400/50 h-52 drop-shadow-lg ease-in hover:scale-125 duration-300 hover:z-10"
        >
          <img class="w-8 md:w-12 mr-4" src="../assets/wind-speed-icon.jpg" alt="" />
          <p class="text-black font-bold text-sm md:text-2xl">
            {{ windSpeed }}
          </p>
        </div>
      </div>
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
#mainContainer.rainy {
  background-image: url("../assets/rain.jpg");
  background-size: 100%;
  background-repeat: no-repeat;
  border-radius: 10px;
}
#mainContainer.sunny {
  background-image: url("../assets/sunny.jpg");
  background-size: 100%;
  background-repeat: no-repeat;
  border-radius: 10px;
}
/* Media Query */
@media screen and (max-width: 600px) {
  #mainContainer.rainy {
    background-image: url("../assets/rain.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    border-radius: 10px;
  }
  #mainContainer.sunny {
    background-image: url("../assets/sunny.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    border-radius: 10px;
  }
}
</style>
