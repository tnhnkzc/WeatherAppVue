<template>
  <div>
    <h1>{{ title }}</h1>
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
