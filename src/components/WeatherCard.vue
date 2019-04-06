<template>
  <v-card height="500">
    <v-card-text>
      <span class="weather-title">{{period.name}}</span> {{period.temperature}}F
    </v-card-text>
    <v-img max-height="150" max-width="150" v-bind:src="period.icon" class="ma-4"></v-img> 
    <v-card-text>{{period.detailedForecast}}</v-card-text>
  </v-card>
</template>

<script>
  export default {
    data: () => ({
    }),
    props: {
      period: Object
    }
  }
</script>

<style>
  .weather-title {
    font-weight: bold;
  }
</style>
