<template lang="html">
  <v-container>
    <v-row >
      <v-col align='center'>
        <p class='display-1'>Weather App</p>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title>Neils Weather App</v-card-title>
          <v-card-subtitle>Please enter your city</v-card-subtitle>
          <v-form>
            <v-container>
              <v-row>
                <v-col>
                  <v-text-field v-model='city' placeholder='Enter a City'></v-text-field>
                  <v-btn v-on:click='getData'>Submit</v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
          <v-row>
            <v-col>
              <v-card-text class='body-1' v-if='info === null'>Please enter a city to get a forecast</v-card-text>
              <v-card-text v-if='info != null' class='display-1'>{{info.weather[0].description}} <v-icon>{{weatherConditionIcons[0].cloud}}</v-icon></v-card-text>
              <!-- <v-btn v-on:click='timeFunction'>Get</v-btn> -->
              <v-card-text class='display-4' v-if='info != null'>{{Math.round(info.main.temp - 273.15)}}°</v-card-text>
              <!-- Below needs to be done after the call to the api.  -->
              <v-icon v-if='time < sunrise'>{{timeOfDayIcons.night}}</v-icon>
              <v-icon v-if='time > sunrise && time < sunset'>{{timeOfDayIcons.day}}</v-icon>
              <v-icon v-if='time > sunset'>{{timeOfDayIcons.night}}</v-icon>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    info: null,
    city: 'Dublin,ie',
    cities: [
      'Dublin',
      'London',
      'New York',
      'Paris',
      'Berlin',
      'Viena',
      'Madrid',
      'Oslo',
      'San Francisco',
      'Portsmouth'
    ],
    timeOfDayIcons: {
      day: "mdi-weather-sunny",
      night: "mdi-moon-new",
    },
    weatherConditionIcons: [
      {
        cloud: 'mdi-cloud-outline',
        code: 100
      },
      {
        cloud: 'mdi-flash',
        code: 100
      },
      {
        cloud: 'mdi-snowflake',
        code: 100
      },
      {
        cloud: 'mdi-weather-sunny',
        code: 100
      },
      {
        cloud: 'mdi-weather-windy',
        code: 100
      },
      {
        cloud: 'mdi-weather-pouring',
        code: 100
      },
    ],
    value: null,
    sunset: {},
    sunrise: {},
    time: {},
    errors: [],

  }),

  methods: {
    getData: function() {
      axios.get(`https://community-open-weather-map.p.rapidapi.com/weather`, {
          params: {
            q: this.city,

          },
          headers: {
            'x-rapidapi-key': '42d6dfc595mshb4a08a510dffa91p1acd3ejsn2b8e894442c0',
            'x-rapidapi-host': 'community-open-weather-map.p.rapidapi.com'
          }
        })
        .then(response => {
          this.info = response.data;
          this.sunset = this.info.sys.sunset;
          this.sunrise = this.info.sys.sunrise;
          this.time = this.info.dt;
        })
        .catch(e => {
          this.errors.push(e)
        })

    },
    getCity: function () {
      console.log(this.city);
    },

    timeFunction: function() {
      this.sunset = this.info.sys.sunset;
      this.sunrise = this.info.sys.sunrise;
      this.time = this.info.dt;
    }
  },

}
</script>

<style lang="css" scoped>
</style>
