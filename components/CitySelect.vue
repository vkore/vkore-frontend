<template>
  <v-select
    :items="cities"
    item-text="title"
    item-value="id"
    title="Города"
    multiple
  />
</template>
<script>
export default {
  name: 'CitySelect',
  data() {
    return {
      cities: []
    }
  },
  mounted() {
    console.log('GETCITIES')
    this.getCities()
  },
  methods: {
    getCities() {
      this.$axios
        .$get('http://localhost:8080/api/get_cities')
        .then((response) => {
          console.log('CIIES', response)
          this.cities = response
        })
    }
  }
}
</script>
