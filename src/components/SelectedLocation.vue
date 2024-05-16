<template>
  <section class="locate">
    <img src="@/assets/location-icon.svg" alt="">
    <p>{{city}}, {{country}}</p>
  </section>
</template>

<script>
export default {
  props: ['getLocation'],
  mounted(){
    this.setCountryAndCity();
  },
  data(){
    return {
      city: '',
      country: '',
    };
  },
  methods:{
    setCountryAndCity(){
      this.getLocation().then( location => {
        this.city = location.city;
        this.country = location.country_name;
      });
    },
  },
};
</script>

<style scoped>
.locate{
  display: flex;
  font-size: 24px;
  align-items: center;
}
.locate p{
  padding-left: 16px;
}
</style>