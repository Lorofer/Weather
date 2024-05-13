<template>
  <MainWeather
      v-bind:city="this.city"
      v-bind:country="this.country"
      v-bind:currentWeatherData="this.getCurrentWeatherData()"
      style="margin: 16px"
  />
</template>

<script>
import MainWeather from "@/components/MainWeather.vue";

export default {
  components: {
    MainWeather,
  },
  data() {
    return {
      city: '',
      country: '',
    };
  },
  mounted() {
    this.getCityAndCountry();
  },
  methods: {
    createUrl(url, params){
      for(let value of Object.entries(params)){
        url.searchParams.append(value[0], value[1].toString());
      }

      return url;
    },
    getLocation() {
      const url = this.createUrl(
          new URL('https://api.ipgeolocation.io/ipgeo'),
          {
            apiKey: '0b80c8d936654520b997bdefc87950c8',
          });

      return fetch(url)
          .then(response => response.json());
    },
    getCurrentWeatherData() {
      return this.getLocation().then(location => {
        const url = this.createUrl(
            new URL('https://api.openweathermap.org/data/2.5/weather'),
            {
              lat: location.latitude.toString(),
              lon: location.longitude.toString(),
              appid: 'eda0abb3cc57eecb8bb542461e309089',
              units: 'metric',
            });

        return fetch(url)
            .then(response => response.json());
      });
    },
    getCityAndCountry(){
      return this.getLocation().then(location => {
        this.city = location.city;
        this.country = location.country_name;
      });
    },
  },
}
</script>

<style scoped>

</style>