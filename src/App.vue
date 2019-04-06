<template>
  <v-app>
    <v-toolbar class="cyan lighten-3">
      <v-toolbar-title>Just Give Me The Weather</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-text-field label="US ZIP Code" v-model="zip" v-on:keyup="keyHandler()"></v-text-field>
      </v-toolbar-items>
    </v-toolbar>
    <v-content>
      <v-container>
        <v-layout row wrap>
          <v-flex xs2 v-for="period in periods" :key="period.name">
            <WeatherCard v-bind:period="period" />
          </v-flex> 
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import WeatherCard from './components/WeatherCard'
import getCoordinates from './zips.js'

export default {
  name: 'App',
  components: {
    WeatherCard
  },
  data: function() {
    return {
      periods:[],
      zip:""
    }
  },
  methods: {
    getWeather: async function(coords) {
      const WEATHER_API = "https://api.weather.gov/points/";
      let full_url = WEATHER_API + coords + "/forecast";
      console.log(full_url)
      let response = await fetch(full_url, { mode: 'cors' });
      let json = await response.json();
      this.periods = json.properties.periods;
    },
    keyHandler: async function() {
      if (this.zip.length == 5) {
        let coords = getCoordinates(this.zip);
        this.getWeather(coords);
      }
    }
  },
  mounted: async function() {
    let context = this;
    navigator.geolocation.getCurrentPosition(function(location) {
      context.getWeather(`${location.coords.latitude},${location.coords.longitude}`);
    });
  }
}
</script>
