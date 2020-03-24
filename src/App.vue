<template>
  <div id="app">
    <label for="inputSearch" >Type to filter options</label>
    <InputSearch v-bind:list="items" id="autocomplete" name="inputSearch" placeholder="Search Items" checktop="true" />
  </div>
</template>

<script>
import InputSearch from './components/InputSearch.vue'
import mockData from './assets/mockData.js'

export default {
  name: 'app',
  components: {
    InputSearch
  },
  data: function() {
  return {
    items: mockData
  }
  }
}
</script>

<style>
label {
  margin-bottom: .5em;
  display: inline-block;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
