<template>
  <h1 class="title">Cities weather</h1>
  <article class="panel is-primary">
    <div class="panel-heading"><h2>{{cityName}}</h2></div>
    <div class="panel-block">
      <l-map :zoom="13" :lat="cityLatitude" :long="cityLongitude"/>
    </div>
    <div class="panel-block">
      <div class="control">
        <label class="radio">
          <input type="radio" name="degree" value="C" v-model="degree">
          C°
        </label>
        <label class="radio">
          <input type="radio" name="degree" value="F" v-model="degree">
          F°
        </label>
      </div>
    </div>
    <div class="panel-block">
      <table class="table">
        <tr>
          <th>Day</th>
          <th>Weather</th>
          <th>Max</th>
          <th>Min</th>
        </tr>
        <tr v-for="weatherPerDay of weather" :key="weatherPerDay">
          <td>{{weatherPerDay?.date}}</td>
          <td>{{weatherPerDay?.weather}}</td>
          <td>{{displayInDegree(weatherPerDay?.temp2m.max)}} {{degree}}°</td>
          <td>{{displayInDegree(weatherPerDay?.temp2m.min)}} {{degree}}°</td>
        </tr>
      </table>
    </div>
    <div class="panel-block">
      <router-link to="/" class="button is-rounded">
        Go back home
      </router-link>
    </div>
  </article>
</template>
<script>
import API from "@/api/weather.api";
import LMap from "@/components/LMap";

export default {
  name: 'City',
  components: {
    LMap
  },
  props: {
    cityName: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      degree: 'C',
      weather: null
    }
  },
  methods: {
    displayInDegree(temperature) {
      return this.degree === 'C' ? temperature :  temperature * (9/5) + 32
    }
  },
  computed: {
    cityLatitude() { return this.$store.getters.getCityPosition(this.cityName)[0] },
    cityLongitude() { return this.$store.getters.getCityPosition(this.cityName)[1] }
  },
  created() {
    API.getCityNextWeekWeather(this.cityLongitude, this.cityLatitude).then(res => this.weather = res)
  }
}
</script>
