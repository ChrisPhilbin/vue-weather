<template>
  <div
    v-for="day in forecastData"
    :key="day.startTime"
    class="grid grid-cols-12 max-w-5xl ml-auto mr-auto px-3 py-3 even:bg-purple-50"
  >
    <div class="text-left col-span-2">
      <p class="text-xl font-semibold font-sans">
        {{ day.name }}
      </p>
    </div>

    <div class="col-span-10 text-left ml-5">
      <p class="text-gray-700 font-sans">
        {{ day.detailedForecast }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ForecastTable",
  props: ["forecastData"],
  data() {
    return {};
  },
};
</script>
