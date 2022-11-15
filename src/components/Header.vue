<template>
  <div class="flex justify-center">
    <img class="w-64 drop-shadow-lg" src="../assets/weatherLogo.webp" alt="weather logo" />
  </div>
  <div class="text-center"></div>
</template>

<script>
export default {
  name: "HeaderComponent",
  props: {
    title: String,
  },
};
</script>
