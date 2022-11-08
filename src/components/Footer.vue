<template>
  <footer class="bottom-0 left-0 fixed text-center w-screen">
    <div class="flex flex-col items-center">
      <span class="flex gap-2"
        >Based on data from
        <a href="https://openweathermap.org/">
          <img class="w-12" src="../assets/openweather.png" alt="" />
        </a>
      </span>
      Tunahan Kuzucu &copy; {{ year }}
      <a href="https://www.github.com/tnhnkzc">GitHub</a>
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
      console.log(year);
    },
  },
  beforeMount() {
    this.getYear();
  },
};
</script>
