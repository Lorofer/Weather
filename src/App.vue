<template>
  <SelectedLocation
      v-bind:getLocation="this.getLocation"
  />
  <MainWeather
      v-bind:createUrl="this.createUrl"
      v-bind:getLocation="this.getLocation"
  />
</template>

<script>
import MainWeather from "@/components/MainWeather.vue";
import SelectedLocation from "@/components/SelectedLocation.vue"

export default {
  components: {
    MainWeather, SelectedLocation,
  },
  mounted() {
    this.getLocation = this.cachingDecorator(this.getLocation);
    this.createUrl = this.cachingDecorator(this.createUrl);
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
    cachingDecorator(func) {
      let cache = new Map();

      return function(x) {
       if (cache.has(x)) {
         return cache.get(x);
        }

        let result = func.call(this, x);
        cache.set(x, result);

        return result;
      };
    },
  },
}
</script>

<style>

</style>