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
                  <v-autocomplete
                    vmodel='null'
                    :items = 'cities'
                    filled
                    label='Enter your city'
                  ></v-autocomplete>
                  <v-btn v-on:click='getForecast'>Submit</v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
          <v-row>
            <v-col>
              <v-card-text v-if='info != null'><v-icon>mdi-cloud-outline</v-icon> {{info.list[0].weather[0].main}}</v-card-text>

              <v-card-text class='body-1' v-if='info === null'>Please enter a city to get a forecast</v-card-text>
              <v-card-text class='display-4' v-if='info != null'>{{Math.round(info.list[0].main.temp - 273.5)}}°</v-card-text>
              <v-card-text v-if='info != null'>{{info.list[0].dt_txt}}</v-card-text>
              <v-icon x-large color='yellow'>{{timeOfDayIcons.day}}</v-icon>
            </v-col>
            <v-col>

              <v-icon></v-icon>
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
  data : () => ({
    info: null,
    cities: [
      'Dublin',
      'London',
      'New York',
      'Paris',
      'Berlin',
    ],
    timeOfDayIcons: {
      day: "mdi-weather-sunny",
      night: "mdi-moon-new",
    }


    ,
    weatherConditionIcons: [

    ],
    value: null,
    temperature: {temp: 100},
    date:  "01/09/20",
    time: "20:07",
    errors: [],
    city: "posts",

  }),
methods: {
  getForecast: function() {
    axios.get(`https://rapidapi.p.rapidapi.com/forecast`,
      {
      params:{
        q: 'Dublin,ie'
      },
      headers: {
        'x-rapidapi-key': '42d6dfc595mshb4a08a510dffa91p1acd3ejsn2b8e894442c0',
        'x-rapidapi-host': 'community-open-weather-map.p.rapidapi.com'}
      }
  )
      .then(response => {
        this.info = response.data
      })
      .catch(e => {
        this.errors.push(e)
      })
      console.log(this.info);
  },
  getQ: function (city) {
    axios.get(``)
  }
}


}


</script>

<style lang="css" scoped>
</style>
