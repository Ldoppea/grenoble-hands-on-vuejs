<template>
  <h1 class="title">Cities weather</h1>
  <article class="panel is-primary">
    <div class="panel-heading"><h2>{{cityName}}</h2></div>
    <div class="panel-block">Weather: {{ weather?.weather }}</div>
    <div class="panel-block">Max: {{ weather?.temp2m.max }}</div>
    <div class="panel-block">Min: {{ weather?.temp2m.min }}</div>
    <div class="panel-block">
      <l-map :zoom="13" :lat="cityLatitude" :long="cityLongitude"/>
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
      weather: null
    }
  },
  computed: {
    cityLatitude() { return this.$store.getters.getCityPosition(this.cityName)[0] },
    cityLongitude() { return this.$store.getters.getCityPosition(this.cityName)[1] }
  },
  created() {
    API.getCityTodayWeather(this.cityLongitude, this.cityLatitude).then(res => this.weather = res)
  }
}
</script>
