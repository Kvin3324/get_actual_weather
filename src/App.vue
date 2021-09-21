<template>
  <div id="app">
    <InputCity
      :citiesSuggestionsArr="citiesSuggestionsArr"
      :hideSuggestions="hideSuggestions"
      :cityName="citySearched"
      @city-selected='citySelected'
      @city-searched-name="searchedCity"
    />
    <section class="weather">
      <actual-weather-data
        :city="weatherData.name"
        :country="getWeatherCountry"
        :weatherIcon="getWeatherIcon"
        :weatherDescription="getWeatherDescription"
        :weatherDateDay="getWeatherDateDay"
        :weatherDateMonth="getWeatherDateMonth"
        :weatherTemp="getWeatherActualTemp"
      >
      </actual-weather-data>
      <hr/>
      <weather-data
        :getWeatherTempMin="getWeatherTempMin"
        :getWeatherTempMax="getWeatherTempMax"
        :getWeatherHumidity="getWeatherHumidity"
        :getWeatherWind="getWeatherWind"
      >
      </weather-data>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
import InputCity from './components/InputCity.vue'
import WeatherData from './components/WeatherDataAbout.vue';
import ActualWeatherData from './components/ActualWeatherData.vue';

export default {
  name: 'App',
  components: {
    InputCity,
    WeatherData,
    ActualWeatherData
  },

  data() {
    return {
      citySearched: 'Paris',
      lang: 'fr',
      weatherData: {},
      citiesSuggestionsArr: [],
      hideSuggestions: true
    }
  },

  created() {
    this.getWeather(this.citySearched);
  },

  computed: {
    getWeatherCountry() {
      return this.weatherData.sys?.country;
    },
    getWeatherDateDay() {
      return new Intl.DateTimeFormat('FR', { weekday: 'long'}).format();
    },
    getWeatherDateMonth() {
      return new Date().getMonth();
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
    // Function to make API call to have weather thanks to this.citySearched
    async getWeather() {
      try {
        const { data } = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.citySearched}&appid=${process.env.VUE_APP_WEATHER_API_KEY}&lang=${this.lang}&units=metric`);

        this.weatherData = data;
      } catch (error) {
        this.$toast.error("Cette ville n'existe pas.");
      }
    },

    // Function to make API call to get weather's data
    async searchedCity(e) {
      // Condition to check input value and make API call
      if (e !== '') {
        try {
          const { data } = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${e}.json?access_token=${process.env.VUE_APP_TOKEN_MAP_KEY}`);

         this.citiesSuggestionsArr = data.features;
         this.hideSuggestions = false;
         return; // return to exit function and don't reset this.citiesSuggestionsArr
        } catch (error) {
          this.$toast.error("Cette ville n'a pas été trouvée.");
        }
      }

      this.citiesSuggestionsArr = [];
      this.hideSuggestions = true;
    },

    // Function to select city && pick up city's weather data
    async citySelected(city) {
      try {
        const { data } = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city.target.outerText}&appid=${process.env.VUE_APP_WEATHER_API_KEY}&lang=${this.lang}&units=metric`);

        this.weatherData = data;
        this.hideSuggestions = true;
      } catch (error) {
        this.$toast.error("Cette ville n'a pas été trouvée.");
      }
    }
  }
}
</script>

<style lang="scss">
@import "./assets/_variables.scss";

body {
  background: $background_color;
  height: 100vh;
}

#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $principal_color;
  padding: 4%;

  .weather {
    hr {
      background: $principal_color;
      opacity: .2;
    }

    @media screen and (min-width: 320px) {
      margin-top: 18%;

      hr {
        margin: 15% 0;
      }
    }

    @media screen and (min-width: 992px) {
      margin-top: 25%;

      hr {
        margin: 20% 0;
      }
    }
    @media screen and (min-width: 1200px) {
      hr {
        margin: 15% 0;
      }
    }
    @media screen and (min-width: 1400px) {
      margin-top: 15%;
    }
  }

  @media screen and (min-width: 992px) {
    width: 50%;
    margin: auto;
  }
}
</style>
