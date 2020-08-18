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
  methods: {
    async loadMembers() {
      const groups = this.selected
        .filter((g) => {
          return typeof g === 'object' || typeof g === 'string'
        })
        .map((g) => {
          if (typeof g === 'object') {
            return g.screen_name
          } else if (typeof g === 'string') {
            return g
          }
        })

      const { data } = await this.$axios.post(
        'http://localhost:8080/api/load_groups',
        groups
      )
      console.log('DATA', data)
    }
  }
}
</script>
