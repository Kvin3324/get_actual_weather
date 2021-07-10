<template>
  <div class="input--search">
    <label>Recherche la météo de:</label>
    <input @blur="cityValue" placeholder="Paris"  />
  </div>
</template>

<script>
export default {
  name: 'input-city',

  methods: {
    cityValue(e) {
      this.$emit('city-value', { e });
    }
  }

}
</script>

<style lang='scss' scoped>
.input--search {
  display: flex;
  align-items: flex-start;
  flex-direction: column;
  margin-top: 30px;

  label {
    margin-bottom: 10px;
  }

  input {
    font-weight: 200;
    border: none;
    border-bottom: solid 1px white;
    color: white;
    padding: 5px;
    background: #232E50;

    &:focus {
      border: solid 2px #0F6EFD;
      outline: none;
    }
  }
}
</style>
