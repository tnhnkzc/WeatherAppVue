<template>
  <footer class="bottom-0 left-0 text-center w-screen mt-8">
    <div class="flex flex-col items-center">
      <span class="flex gap-2"
        >Based on data from
        <a href="https://openweathermap.org/">
          <img class="w-12" src="../assets/openweather.png" alt="" />
        </a>
      </span>
      Tunahan Kuzucu &copy; {{ year }}
      <a href="https://www.github.com/tnhnkzc/WeatherAppVue">GitHub</a>
    </div>
  </footer>
</template>

<script>
export default {
  name: "FooterComponent",
  data() {
    return {
      year: "",
    };
  },
  methods: {
    async getYear() {
      this.year = new Date().getFullYear();
    },
  },
  beforeMount() {
    this.getYear();
  },
};
</script>
