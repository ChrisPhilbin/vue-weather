<template>
  <div
    v-for="day in forecastData"
    :key="day.startTime"
    class="grid grid-cols-12 max-w-5xl ml-auto mr-auto px-3 py-3 even:bg-blue-100 rounded"
  >
    <div class="text-left col-span-3">
      <p class="text-sm lg:text-xl text-gray-600 font-semibold font-sans">
        {{ day.name }}
      </p>
    </div>

    <div class="col-span-9 text-left ml-5">
      <p class="text-gray-600 font-sans">
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
