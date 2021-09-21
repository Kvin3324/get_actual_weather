<template>
  <div class="weather__data__actual">
    <div class="weather__data__actual--city">
      <h4> {{ city }}, {{ country }}</h4>
    </div>
    <div class="weather__data__actual--temp">
      <img :src="`https://openweathermap.org/img/w/${weatherIcon}.png`" />
      <div class="weather__data__actual--temp--about">
        <h4>{{ weatherTemp }}°</h4>
        <p class="weather--description">{{ weatherDescription }}</p>
        <p class="weather--date">{{ weatherDateDay }} {{ weatherDateMonth }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'actual-weather-data',
  props: {
    city: {
      type: String,
      default: ''
    },
    country: {
      type: String,
      default: ''
    },
    weatherIcon: {
      type: String,
      default: ''
    },
    weatherTemp: {
      type: Number,
      default: 0
    },
    weatherDescription: {
      type: String,
      default: ''
    },
    weatherDateDay: {
      type: String,
      default: ''
    },
    weatherDateMonth: {
      type: Number,
      default: 0
    },
  }
}
</script>

<style lang='scss' scoped>
  .weather__data__actual {
    display: flex;
    margin-top: 10px;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    img {
      margin-right: 10px;
      width: inherit;
      max-width: 150px;
    }

    &--temp {
      width: 100%;
      &--about {
        h4 {
          font-size: 4em;
          margin: 0;
          margin-left: 20px;
        }
        p {
          margin: 0;
        }
        .weather--description {
          font-weight: bold;
        }
        .weather--date {
          opacity: .5;
          font-size: .9em;
        }
      }
    }

    &--city {
      display: flex;
      flex-direction: column-reverse;

      h4 {
        font-weight: bold;
      }
    }

    @media screen and (min-width: 320px) {
      &--temp {
        width: 95%;
      }
    }

    @media screen and (min-width: 576px) {
      &--temp {
        width: 92%;
      }
    }

    @media screen and (min-width: 768px) {
      &--city {
        h4 {
          font-size: 2.5em;
        }
      }

      &--temp {
        width: 70%;

        &--about {
          h4 {
            font-size: 5.5em;
          }
          .weather--description {
            font-size: 1.5em;
          }
          .weather--date {
            font-size: 1.3em;
          }
        }
      }
    }
    @media screen and (min-width: 992px) {
      &--temp {
        width: 60%;
      }
    }

    @media screen and (min-width: 1400px) {
      &--temp {
        width: 50%;
      }
    }
  }
</style>