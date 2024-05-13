<template>
  <section>
    <div class="locate">
      <img src="@/assets/location-icon.svg" alt="">
      <p>{{city}}, {{country}}</p>
    </div>
    <div class="icon">
      <img src="@/assets/WeatherIcon%20-%202-32.svg">
    </div>
    <div class="weatherData">
      <h1>{{currentTemperature}}</h1>
      <p class="weatherStatus"><strong>{{currentWeatherStatus}}</strong></p>
      <p class="feelsLike">{{feelsLike}}</p>
    </div>
    <div class="dateAndTime">
      <p>{{date}}</p>
      <p>{{day}} | {{time}}</p>
    </div>
  </section>
</template>

<script>
export default {
  data(){
    return{
      currentTemperature: '',
      currentWeatherStatus: '',
      feelsLike: '',

      date: '',
      day: '',
      time: '',
    };
  },
  props: ['city', 'country', 'currentWeatherData'],
  mounted(){
    this.getDateAndTime();
    this.setWeather();
  },
  methods: {
    setWeather(){
      this.currentWeatherData.then(weather => {
        this.currentTemperature = `${Math.round(weather.main.temp)}°`;
        this.currentWeatherStatus = weather.weather[0].main;
        //this.currentWeatherStatus = weather.weather[0].description;
        this.feelsLike = `Feels like ${Math.round(weather.main.feels_like)}°`;
      });
    },
    getDateAndTime(){
      const dateObj = new Date();

      let date = dateObj.getDate().toString();
      let month = dateObj.getMonth();
      let year = dateObj.getFullYear();

      let day = dateObj.getDay();

      let hours = dateObj.getHours();
      let minutes = dateObj.getMinutes();

      if(
          (date[date.length - 1] > 0) &&
          (date[date.length - 1] < 4) &&
          (date < 11 || date > 13)
      ){
        switch(date[date.length - 1]){
          case(1):
            date += 'st';
            break;
          case(2):
            date += 'nd';
            break;
          case(3):
            date += 'rd';
            break;
        }
      } else {
        date += 'th';
      }

      const monthsArray = [
        'Jan', 'Feb', 'Mar', 'Apr',
        'May', 'June', 'Jul', 'Aug',
        'Sept', 'Oct', 'Nov', 'Dec',
      ];
      month = monthsArray[month];

      year = `‘${year % 1000}`;

      const daysArray = [
        'Sunday', 'Monday', 'Tuesday',
        'Wednesday', 'Thursday', 'Friday',
        'Saturday',
      ];
      day = daysArray[day];

      if(hours < 10){
        hours = '0' + hours.toString();
      }

      if(minutes < 10){
        minutes = '0' + minutes.toString();
      }

      this.date = `${date} ${month} ${year}`;
      this.day = day;
      this.time = `${hours}:${minutes}`;
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
};
</script>

<style scoped>
*{
  font-family: "Inter", sans-serif;
  color: white;
  font-weight: normal;
}
.locate{
  display: flex;
  font-size: 24px;
  align-items: center;
}
.locate p{
  padding-left: 16px;
}

.icon{
  margin-top: 24px;
  width: 128px;
}
.icon img{
  width: 100%;
  height: 100%;
}

.weatherData{
  margin-top: 24px;
}
.weatherData h1{
  font-size: 128px;
  line-height: 0.8;
}
.weatherData .weatherStatus{
  margin-top: 14px;
  font-size: 24px;
}
.weatherData .feelsLike{
  margin-top: 6px;
  font-size: 20px;
}

.dateAndTime{
  font-size: 24px;
  margin-top: 24px;
  font-weight: bold;
}
.dateAndTime p:last-child{
  font-size: 20px;
  margin-top: 6px;
  word-spacing: 12px;
}
</style>