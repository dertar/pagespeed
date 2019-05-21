<template>
  <v-flex d-flex xs12 style="margin: auto;">
    <v-layout row>
      <v-flex d-flex xs1>
        <v-icon
          :class="cstyle"
        >mdi-weather-cloudy</v-icon>
      </v-flex>
      <v-flex d-flex xs10 offset-xs1>{{ title }}</v-flex>
    </v-layout>
  </v-flex>
</template>

<script>
export default {
    props:{
        cstyle: {
            required: true
        },
        title: {
            type: String,
            required: true
        }
    }
};
</script>

<style>
</style>
