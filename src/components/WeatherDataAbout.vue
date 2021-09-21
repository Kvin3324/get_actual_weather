<template>
  <div class="weather__data__about">
    <div class="weather__data__about--variants">
      <p class="label--about">max/min</p>
      <p class="temp--data">{{ getWeatherTempMax }}° / {{ getWeatherTempMin }}°</p>
    </div>
    <div class="weather__data__about--humidity">
      <p class="label--about">Taux d'humidité</p>
      <p class="humidity--data">{{ getWeatherHumidity }}%</p>
    </div>
    <div class="weather__data__about--wind">
      <p class="label--about" >Vent</p>
      <p class="wind--data">{{ getWeatherWind }}km/h</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'weather-data',
  props: {
    // Props to have weather details data

    getWeatherTempMin: {
      type: Number,
      default: 0
    },
    getWeatherTempMax: {
      type: Number,
      default: 0
    },
    getWeatherHumidity: {
      type: Number,
      default: 0
    },
    getWeatherWind: {
      type: Number,
      default: 0
    },
  }
}
</script>

<style lang='scss' scoped>
  .weather__data__about {
    display: flex;
    justify-content: space-between;
    font-size: .8em;

    p {
      padding: 4px;
      margin: 0;
    }

    &--variants, &--humidity, &--wind {
      display: flex;
      flex-direction: column-reverse;
      line-height: .7;

      .temp--data, .humidity--data, .wind--data {
        font-weight: bold;
      }

      .label--about {
        opacity: .6;
      }
    }

    @media screen and (min-width: 768px) {
      div {
        font-size: 1.3em;
      }
    }

    @media screen and (min-width: 992px) {
      div {
        font-size: 1.5em;
      }
    }
  }
</style>