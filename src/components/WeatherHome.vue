<template>
  <div class="pb-10">
    <p class="text-4xl text-blue-400 font-sans uppercase">
      Welcome to the Vue Weather App
    </p>
    <form @submit.prevent="getLatLonFromOpenWeather">
      <input
        type="text"
        v-model="zip"
        maxlength="5"
        placeholder="enter zip code..."
        class="shadow border rounded py-2 px-3 text-gray-700 mt-6"
      />
      <button
        @click="getLatLonFromOpenWeather"
        class="bg-blue-500 text-white rounded p-2 m-2"
      >
        Go!
      </button>
    </form>
    <current-conditions
      v-if="Object.keys(currentObservations).length"
      :currentObservations="currentObservations"
      :city="city"
      :coords="coords"
      :alerts="alerts"
    />
    <div v-if="forecastData.length">
      <p class="text-2xl text-blue-400 font-sans pt-10">
        A look ahead in {{ city }}
      </p>
      <div
        class="grid grid-cols-1 lg:grid-cols-9 gap-3 max-w-5xl rounded shadow-lg relative mr-auto ml-auto mb-8 py-8 px-6"
      >
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
  </div>
</template>

<script>
import CurrentConditions from "./CurrentConditions.vue";
export default {
  name: "WeatherHome",
  components: {
    CurrentConditions,
  },
  data() {
    return {
      alerts: [],
      city: "",
      coords: {},
      currentObservations: {},
      errors: false,
      forecastURL: "",
      forecastData: [],
      forecastBlocks: [],
      zip: "",
      zone: "",
    };
  },
  methods: {
    async getLatLonFromOpenWeather() {
      try {
        let response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?units=imperial&zip=${this.zip},us&appid=${process.env.VUE_APP_OPEN_WEATHER_API_KEY}`
        );
        let data = await response.json();
        if (response.ok) {
          this.coords = {
            lat: data.coord.lat,
            lon: data.coord.lon,
          };
          this.currentObservations = data;
          this.city = data.name;
          this.getForecastFromLatLon();
          document.title = `Currently in ${this.city}`;
        }
      } catch (error) {
        this.errors = true;
        console.log(error, "Something went wrong!");
      }
    },
    async getForecastFromLatLon() {
      try {
        let response = await fetch(
          `https://api.weather.gov/points/${this.coords.lat},${this.coords.lon}`
        );
        let data = await response.json();
        if (response.ok) {
          this.forecastURL = data.properties.forecast;
          this.zone = data.properties.county.slice(-6);
          try {
            let response = await fetch(this.forecastURL);
            let data = await response.json();
            if (response.ok) {
              this.forecastData = data.properties.periods;
              this.forecastBlocks = data.properties.periods.slice(0, 9);
            }
          } catch (error) {
            this.errors = true;
            console.log(error, "Error! Something went wrong!");
          }
          try {
            let response = await fetch(
              `https://api.weather.gov/alerts/active/zone/${this.zone}`
            );
            let data = await response.json();
            if (response.ok) {
              this.alerts = data.features;
              console.log(this.alerts);
            }
          } catch (error) {
            this.errors = true;
            console.log(error, "Error! Something went wrong!");
          }
        }
      } catch (error) {
        console.log(error, "Error. Something went wrong!");
        this.errors = true;
      }
    },
  },
  computed: {},
  created() {},
};
</script>

<style></style>
