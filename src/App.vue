<template>
  <div>
    <Header title="Weather App" />
    <Weather />
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import Weather from "./components/Weather";

export default {
  name: "App",
  components: {
    Header,
    Footer,
    Weather,
  },
};
</script>

<style>
body {
  font-family: "Lato", sans-serif;
  font-family: "Montserrat", sans-serif;
}
</style>
