<template>
  <div class="input--search">
    <input placeholder="Rechercher" v-model="cityName" />
    <div
      id="suggestions"
      class="cities--suggestions"
      v-if="!hideSuggestions"
    >
      <p
        v-for="(city, i) in citiesSuggestionsArr"
        :key="i"
        @click="citySelected"
      >
        {{ city.place_name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'input-city',
  props: {
    hideSuggestions: {
      type: Boolean,
      default: true
    },

    citiesSuggestionsArr: {
      type: Array,
      default: () => []
    },
  },

  data() {
    return {
      cityName: ''
    }
  },

  methods: {
    // Emit function when selecting city
    citySelected(city) {
      this.cityName = ''; // Reset to empty this.cityName which is the input value
      this.$emit('city-selected', city);
    }
  },

  watch: {
    // Watching this.cityName when input value change
    cityName() {
      this.$emit('city-searched-name', this.cityName);
    }
  }

}
</script>

<style lang='scss' scoped>
@import "../assets/_variables.scss";

.input--search {
  display: flex;
  align-items: flex-start;
  flex-direction: column;

  input {
    background: $principal_color;
    display: flex;
    border: 1px solid $border_color;
    box-shadow: none;
    border-radius: 10px;
    z-index: 3;
    height: 44px;
    width: 100%;
    box-shadow: 0 4px 6px rgb(32 33 36 / 28%);
    padding-left: 10px;

    &:focus {
      border: solid 2px $blue_color;
      outline: none;
    }
  }

  .cities--suggestions {
    display: flex;
    flex-direction: column;
    width: 100%;

    p {
      background: $principal_color;
      color: $grey_color;
      text-align: left;
      margin: 0;
      padding: 10px 5px;
      padding-left: 5px;
      box-shadow: 0 0 10px $principal_shadow_color;
      border-bottom: 1px solid $secondary_shadow_color;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }
}
</style>
