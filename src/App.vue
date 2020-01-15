<template lang="html">
  <div>
    <h1>Countries</h1>
    <country-filter-form :countries="countries" :parentAlpha3Code="currentAlpha3Code"/>
    <country-detail :country="selectedCountry"/>
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
      currentAlpha3Code: "GBR"
    };
  },
  components: {
    "country-filter-form": CountryFilterForm,
    "country-detail": CountryDetail
  },
  computed: {
    selectedCountry: function(){
      return this.countries.find(country => country.alpha3Code === this.currentAlpha3Code)
    }
  },
  mounted: function() {
    eventBus.$on('search-entered', nameToSearch => this.search(nameToSearch))

    eventBus.$on('country-selected', (alpha3Code) => {
      this.currentAlpha3Code = alpha3Code
    })

    fetch("https://restcountries.eu/rest/v2/all")
      .then(res => res.json())
      .then(countries => {
        this.countries = countries
      })
  },
  methods: {
    search: function(nameToSearch) {
      let foundCountry = this.countries.find(country => {
        return (
          country.name.toLowerCase().indexOf(nameToSearch.toLowerCase()) > -1
        );
      });
      if (!foundCountry) return;
      this.currentAlpha3Code = foundCountry.alpha3Code;
    }
  }
};
</script>

<style lang="css" scoped>
h1 {
  text-align: center;
  color: #333;
}
</style>
