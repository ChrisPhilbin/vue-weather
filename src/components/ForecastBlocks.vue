<template>
  <div v-if="forecastBlocks.length">
    <div
      class="grid grid-cols-1 lg:grid-cols-9 gap-3 max-w-5xl rounded-md shadow-lg relative mr-auto ml-auto  my-8 py-8 px-6 bg-white"
    >
      <div class="col-span-full">
        <p class="text-2xl text-blue-400 font-sans">
          A look ahead in {{ city }}
        </p>
      </div>
      <div
        v-for="forecast in forecastBlocks"
        :key="forecast.name"
        class="w-28 ml-auto mr-auto text-center"
      >
        <p class="text-center text-xs font-bold pb-4 font-sans text-gray-600">
          {{ forecast.name }}
        </p>
        <img
          :src="forecast.icon"
          :alt="forecast.shortForecast"
          class="mx-auto rounded"
        />
        <p class="text-center text-xs pt-4 font-sans text-gray-600">
          {{ forecast.shortForecast }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ForcastBlocks",
  props: ["city", "forecastBlocks"],
  data() {
    return {};
  },
};
</script>
