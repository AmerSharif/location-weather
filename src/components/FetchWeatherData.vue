<template>
    <div class="row">
        <div class="col-md-12">
            <h3>Locations</h3>
        </div>
        <div class="col-md-12">
            <table>
                <tr>
                    <td>Nr</td>
                    <td>Stasjon</td>
                    <td>Temperatur</td>
                    <td>Precipitation</td>
                    <td>Dato</td>
                </tr>
                <tr v-for="(weather, index) in locationWeather" :key="index">
                    <td>{{index + 1}}</td>
                    <td>{{weather.name}}</td>
                    <td>{{weather.temperature}}</td>
                    <td>{{weather.precipitation}}</td>
                    <td></td>
                </tr>
            </table>
        </div>
        <div class="row mt-3">
            <div class="col-md-12 text-center">
                <button v-for="(location, index) in locations" :key="index" @click="fetchLocationData(location.latitude, location.longitude)" class="btn btn-md btn-primary">location.name</button>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col-md-12 text-center">
                <button @click="fetchLocationData('59.9440475', '10.8309479')" class="btn btn-md btn-primary">Linderud</button>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col-md-12 text-center">
                <button @click="fetchLocationData('59.904074', '10.821857')" class="btn btn-md btn-primary">Bryn</button>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col-md-12 text-center">
                <button @click="fetchAllLocationsWeatherData" class="btn btn-md btn-primary">Hent alle</button>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'
    interface LocationWeather {
        name: string,
        temperature: number,
        precipitation: number
    }
    interface Locations {
        name: string,
        latitude: string,
        longitude: string
    }
export default defineComponent({
  name: 'LocationWeather',
  data () {
    return {
      locationWeather: [] as LocationWeather[],
      locations: [] as Locations[],
      fetchingFacts: false
    }
  },
  methods: {
    async fetchLocations () {
      const locationsResponse = await axios.get<Locations[]>('[domain]/api/locations')
      this.locations = locationsResponse.data
      this.locations.push(...(locationsResponse.data || []))
    },
    async fetchAllLocationsWeatherData () {
      const locationWeatherResponse = await axios.get<LocationWeather[]>('[domain]/api/locationweather')
      this.locationWeather = locationWeatherResponse.data

      this.fetchingFacts = false
    },
    async fetchLocationData (latitude: string, longitude: string) {
      this.fetchingFacts = true
      const locationWeatherResponse = await axios.get<LocationWeather[]>('[domain]/api/locationweather/' + latitude + '/' + longitude)
      this.locationWeather = locationWeatherResponse.data

      this.fetchingFacts = false
    },
    async mounted () {
      // Todo: fix this. Why is'nt it working?
      await this.fetchLocations()
    }
  }
})
</script>
