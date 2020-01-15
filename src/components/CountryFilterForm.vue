<template lang="html">
  <form v-on:submit.prevent>
    <input type="text" v-model="search" placeholder="search for country...">
    <select v-model="selectedAlpha3Code">
      <option disabled value="">Select a country...</option>
      <option v-for="country in countries" :value="country.alpha3Code">{{country.name}}</option>
    </select>
  </form>
</template>

<script>
import { eventBus } from "../main.js";

export default {
  name: "country-filter-form",
  data() {
    return {
      search: "",
      selectedAlpha3Code: this.parentAlpha3Code
    };
  },
  props: ["countries", "parentAlpha3Code"],
  watch: {
    selectedAlpha3Code: function() {
      eventBus.$emit("country-selected", this.selectedAlpha3Code);
    },
    search: function(){
      eventBus.$emit("search-entered", this.search);
    },
    parentAlpha3Code: function(){
      this.selectedAlpha3Code = this.parentAlpha3Code;
    }
  }
};
</script>

<style lang="css" scoped>
form {
  text-align: center;
  margin: 40px 0;
}

select,
input[type="text"] {
  font-size: 18px;
}

select {
  margin-left: 20px;
}
</style>
