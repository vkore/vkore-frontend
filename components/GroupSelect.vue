<template>
  <v-combobox
    v-model="selected"
    :items="groups"
    multiple
    item-text="screen_name"
    item-value="id"
  />
</template>
<script>
export default {
  name: 'GroupSelect',
  data() {
    return {
      groups: [],
      selected: [],
      label: 'Введите или выберите группы для парсинга'
    }
  },
  mounted() {
    this.$axios.get('http://localhost:8080/api/all_groups').then((response) => {
      this.groups = response.data
    })
  },
  methods: {}
}
</script>
