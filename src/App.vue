<template>
  <div id="app">
    <div>
      <h2>Weather App</h2>
    </div>
    <InputCity
      @city-value="renderWeatherCity"
    />
    <section class="weather">
      <div class="weather__data">
        <h4> {{ weatherData.name }}, {{ getWeatherCountry }}</h4>
        <p>{{ new Date().toDateString() }}</p>
        <div class="weather__data__actual--temp">
          <img :src="`https://openweathermap.org/img/w/${getWeatherIcon}.png`" />
          <h3>{{ getWeatherActualTemp }}°</h3>
        </div>
        <p> {{ getWeatherDescription }}</p>
        <div class="weather__data--variants">
          <p><strong>Temp min:</strong> {{ getWeatherTempMin }}°</p>
          <p><strong>Temp max:</strong> {{ getWeatherTempMax }}°</p>
        </div>
        <div class="weather__data--about">
          <div class="weather--data--about--humidity">
            <p><strong>Taux d'humidité:</strong>: {{ getWeatherHumidity }}%</p>
          </div>
          <div class="weather__data--about--wind">
            <p><strong>Vent:</strong>{{ getWeatherWind }}km/h</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Vue from 'vue';
import axios from 'axios';
import InputCity from './components/InputCity.vue'
import VueToast from 'vue-toast-notification';
import 'vue-toast-notification/dist/theme-sugar.css';

export default {
  name: 'App',
  components: {
    InputCity
  },

  data() {
    return {
      citySearched: 'Pellegrue',
      lang: 'fr',
      weatherData: {},
    }
  },

  created() {
    Vue.use(VueToast);

    this.getWeather(this.citySearched);
  },

  computed: {
    getWeatherCountry() {
      return this.weatherData.sys?.country;
    },
    getWeatherIcon() {
      return this.weatherData.weather?.[0].icon
    },
    getWeatherDescription() {
      return this.weatherData.weather?.[0].description;
    },
    getWeatherActualTemp() {
      return Math.round(this.weatherData.main?.temp);
    },
    getWeatherTempMin() {
      return Math.round(this.weatherData.main?.temp_min);
    },
    getWeatherTempMax() {
      return Math.round(this.weatherData.main?.temp_max);
    },
    getWeatherHumidity() {
      return this.weatherData.main?.humidity;
    },
    getWeatherWind() {
      return this.weatherData.wind?.speed;
    },

  },

  methods: {
    cityValue(e) {
      this.citySearched = e.e.target.value;
    },

    async getWeather() {
      try {
        const { data } = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.citySearched}&appid=${process.env.VUE_APP_WEATHER_API_KEY}&lang=${this.lang}&units=metric`);

        this.weatherData = data;
      } catch (error) {
        Vue.$toast.error("Cette ville n'existe pas.");
      }
    },

    renderWeatherCity(e) {
      this.cityValue(e);
      this.getWeather();
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 10px;

  h4, p {
    margin: 10px;
  }

  .weather {
    margin-top: 20%;
    &__data {
      margin-top: 8%;
      &__actual--temp {
        display: flex;
        justify-content: center;
        margin: 8% 0;

        img {
          margin-right: 10px;
        }
      }
      &--variants {
        display: flex;
        justify-content: space-between;
      }
      &--about {
        margin-top: 10%;
      }
    }
  }
}
</style>
