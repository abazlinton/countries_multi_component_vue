<template lang="html">
  <div>
    <h1>Countries</h1>
    <country-filter-form :countries="filteredCountries" :parentAlpha3Code="currentAlpha3Code"/>
    <country-detail :country="renderCountry"/>
  </div>
</template>

<script>
import CountryFilterForm from "./components/CountryFilterForm.vue";
import CountryDetail from "./components/CountryDetail.vue";
import { eventBus } from './main.js'

export default {
  data() {
    return {
      countries: [],
      currentAlpha3Code: "GBR",
      searchTerm: ""
    };
  },
  components: {
    "country-filter-form": CountryFilterForm,
    "country-detail": CountryDetail
  },
  computed: {
    renderCountry: function(){
      return this.countries.find(country => country.alpha3Code === this.currentAlpha3Code)
    },
    filteredCountries: function(){
      const foundCountries = this.countries.filter(country => {
        return country.name.toLowerCase().includes(this.searchTerm.toLowerCase())
      })
      if (foundCountries.length === 0) {
        this.currentAlpha3Code = "???"
        return [{name: "Not Found", alpha3Code: "???"}]
      }
      this.currentAlpha3Code = foundCountries[0].alpha3Code;
      return foundCountries
    }
  },
  mounted: function() {
    eventBus.$on('search-entered', searchTerm => this.searchTerm = searchTerm)

    eventBus.$on('country-selected', (alpha3Code) => {
      this.currentAlpha3Code = alpha3Code
    })

    fetch("https://restcountries.eu/rest/v2/all")
      .then(res => res.json())
      .then(countries => {
        this.countries = countries
      })
  },
};
</script>

<style lang="css" scoped>
h1 {
  text-align: center;
  color: #333;
}
</style>
