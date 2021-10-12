<template>
  <div
    class="grid grid-cols-1 lg:grid-cols-12 relative rounded-md shadow-lg max-w-5xl ml-auto mr-auto mt-8 pb-8 pt-4 bg-white"
  >
    <span
      class="absolute top-5 right-5 bg-red-500 p-2 rounded-md font-sans text-white font-bold text-xs"
      v-if="alerts.length"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5 inline-block mr-1"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path
          fill-rule="evenodd"
          d="M8.257 3.099c.765-1.36 2.722-1.36 3.486 0l5.58 9.92c.75 1.334-.213 2.98-1.742 2.98H4.42c-1.53 0-2.493-1.646-1.743-2.98l5.58-9.92zM11 13a1 1 0 11-2 0 1 1 0 012 0zm-1-8a1 1 0 00-1 1v3a1 1 0 002 0V6a1 1 0 00-1-1z"
          clip-rule="evenodd"
        />
      </svg>
      Active Alerts
      <p></p
    ></span>

    <div class="col-span-3">
      <img
        :src="
          `http://openweathermap.org/img/w/${currentObservations.weather[0].icon}.png`
        "
        class="mr-auto ml-auto"
        width="105"
        height="105"
      />
      <p class="text-gray-600 font-bold font-sans text-xl">
        {{ currentObservations.weather[0].main }}
      </p>
    </div>
    <div class="col-span-4 ml-3 lg:ml-0 text-left">
      <p class="font-bold font-sans tracking-tight text-gray-400 uppercase">
        Currently in {{ city }}
      </p>
      <p class="text-gray-500 font-sans text-sm">
        {{ currentObservations.weather[0].description }}
      </p>
      <p class="text-gray-500 font-sans font-semibold text-xs pb-4">
        Lat: {{ coords.lat }} Lon: {{ coords.lon }}
      </p>
      <p class="text-gray-700 font-sans font-bold text-5xl">
        {{ Math.round(currentObservations.main.temp) }}&deg;F
      </p>
    </div>
    <div class="col-span-5 ml-3 lg:ml-0 text-left">
      <p class="text-sm font-bold font-sans inline-block pr-1">Humidity</p>
      <p class="text-sm font-sans inline-block">
        {{ currentObservations.main.humidity }}%
      </p>
      <br />
      <p class="text-sm font-bold font-sans inline-block pr-1">Wind Speed</p>
      <p class="text-sm font-sans inline-block">
        {{ Math.round(currentObservations.wind.speed) }}mph
      </p>
      <br />
      <p class="text-sm font-bold font-sans inline-block pr-1">Barometer</p>
      <p class="text-sm font-sans inline-block">
        {{ currentObservations.main.pressure }} mb
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "CurrentConditions",
  props: ["currentObservations", "city", "coords", "alerts"],
  data() {
    return {};
  },
  created() {
    console.log("current conditions component created");
  },
  methods: {},
};
</script>
