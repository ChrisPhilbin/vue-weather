<template>
  <div class="text-center bg-blue-50 h-screen">
    <weather-home />
  </div>
</template>

<script>
import WeatherHome from "./components/WeatherHome.vue";

export default {
  name: "App",
  components: {
    WeatherHome,
  },
};
</script>
