<script>
/* eslint-disable no-unused-vars */
import { getWeatherForecastForPosition } from '../util/Api';

const getBackgroundImageClass = (forecast) => {
  const date = new Date(forecast.dt*1000);
  if (19 < date.getHours() || date.getHours() < 6) {
      return 'city-handler--night';
    }
    if (forecast.weather && forecast.weather.length > 0) {
      if (forecast.weather[0].main === 'Sun') {
        return 'city-handler--sun';
      }
      if (forecast.weather[0].main === 'Clear') {
        return 'city-handler--sun';
      }
      if (forecast.weather[0].main === 'Rain') {
        return 'city-handler--rain';
      }
      if (forecast.weather[0].main === 'Snow') {
        return 'city-handler--snow';
      }
      if (forecast.weather[0].main === 'Clouds') {
        return 'city-handler--clouds';
      }
      return 'city-handler--clouds';

    }
    return '';
}
function formatTime(unixDateTimeStamp) {
  const formatedDate = new Date(unixDateTimeStamp*1000);
  const time = `${formatedDate.getHours()}`.length === 1 ? `0${formatedDate.getHours()}:00` : `${formatedDate.getHours()}:00`;
  return time;
}
function formatDate(unixDateTimeStamp) {
  const formatedDate = new Date(unixDateTimeStamp*1000);
  const dateString = formatedDate.getDate() + '/' + formatedDate.getMonth();
  return dateString;
}
function getForecastWithFormatedDateAndTime(forecasts) {
  return forecasts.map(weather => {
      weather.time = formatTime(weather.dt);
      weather.dateString = formatDate(weather.dt);
      return weather;
    });
}
export default {
  name: 'Weather',
  data: () => ({
    cityName: 'Hello World',
    forecastList: [],
    selectedForecast: {},
  }),
  computed: {
   backgroundImage() {
     return getBackgroundImageClass(this.selectedForecast);
   }
  },
  methods: {
    onSelect(forecast) {
      this.selectedForecast = forecast;
    },
  },
  mounted() {
    navigator.geolocation.getCurrentPosition(async ({coords}) => {
      const res = await getWeatherForecastForPosition(coords.latitude, coords.longitude);
      this.cityName = res.city.name;
      this.forecastList = getForecastWithFormatedDateAndTime(res.list);
      this.selectedForecast = this.forecastList[0];
    });
  }
}
</script>

<template>
  <div class="city-handler" :class="backgroundImage">
    <h1 class="header-title">{{ cityName }}</h1>
    <div class="selected-weather">
      <h3>Selected forecast is {{selectedForecast.dateString}} - {{selectedForecast.time}}</h3>
      <span v-if="selectedForecast.main">{{Math.round(selectedForecast.main.temp)}}</span>
    </div>
    <div class="forecast-container">
      <h3>Forecast list</h3>
      <div class="forecast-list">
        <button v-for="forecast in forecastList" @click="onSelect(forecast)" :key="forecast.dt" class="forecast-button">
          <span>
            {{ forecast.dateString }}
          </span>
          <span>
            {{ forecast.time }}
          </span>
          <span>
            {{ Math.round(forecast.main.temp) }}°
          </span>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.city-handler {
  display: grid;
  grid-template-rows: 15% 70% 15%;
  grid-template-columns: 5% 90% 5%;
  height: 100%;
  width: 100%;
  background-size: cover;
  background-repeat:   no-repeat;
  background-position: center center;
  background-image: url('../assets/sunny.jpg');

}

.header-link {
  grid-column-start: 1;
  grid-column-end: 1;
  grid-row: 1;
  padding-top: 10px;
  font-size: 25px;
  padding-left: 10px;
}
.selected-weather h3 {
  font-size: 40px;
  margin: 0
}

.forecast-button {
  height: 60px;
  color: white;
}
.forecast-button:hover {
  color: lightgray;
}

.header-title {
  grid-column-start: 2;
  grid-column-end: 2;
  grid-row: 1;
}


.selected-weather {
  font-size: 40vh;
  grid-row-start: 2;
  grid-row-end: 2;
  grid-column-start: 2;
  grid-column-end: 2;
}


.forecast-container {
  grid-column-start: 2;
  grid-column-end: 2;
}

.forecast-list {
  grid-column-start: 2;
  grid-column-end: 2;
  grid-row: 3;
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  margin-right: 30px;
  overflow-x: auto;
}

.city-handler--sun {
  background-image: url('../assets/sunny.jpg');
}
.city-handler--rain {
  background-image: url('../assets/rain.jpg');
}
.city-handler--snow {
  background-image: url('../assets/snow.jpg');
}
.city-handler--clouds {
  background-image: url('../assets/clouds.jpg');
}
.city-handler--night {
  background-image: url('../assets/night.jpg');
}

</style>
