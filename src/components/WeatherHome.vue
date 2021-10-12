<template>
  <div class="pb-10 pt-8">
    <p class="text-4xl text-blue-400 font-inter-700 uppercase tracking-wide">
      Fast Weather
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

    <forecast-blocks
      v-if="forecastBlocks.length"
      :forecastBlocks="forecastBlocks"
      :city="city"
    />

    <forecast-table v-if="forecastData.length" :forecastData="forecastData" />
  </div>
</template>

<script>
import CurrentConditions from "./CurrentConditions.vue";
import ForecastBlocks from "./ForecastBlocks.vue";
import ForecastTable from "./ForecastTable.vue";
export default {
  name: "WeatherHome",
  components: {
    CurrentConditions,
    ForecastBlocks,
    ForecastTable,
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
