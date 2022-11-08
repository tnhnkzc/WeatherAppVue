<template>
  <div class="flex justify-center">
    <img class="w-64 drop-shadow-lg" src="../assets/weatherLogo.webp" alt="weather logo" />
  </div>
  <div class="text-center">
    <h1 class="text-2xl">{{ title }}</h1>
  </div>
</template>

<script>
export default {
  name: "HeaderComponent",
  props: {
    title: String,
  },
};
</script>
