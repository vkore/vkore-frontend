<template>
  <v-layout column justify-center align-center>
    <v-row>
      <group-select ref="groupSelect" />
      <v-btn @click="loadMembers">Загрузить пользователей</v-btn>
    </v-row>
    <v-row>
      <v-col v-for="u of test" :key="u.id" cols="3">
        <v-card>
          <v-card-title>
            {{ `${u.first_name} ${u.last_name}` }}
          </v-card-title>
          <v-card-text>
            <a :href="getUrl(u.id)" target="_blank">
              <v-img :src="u.photo_200" />
            </a>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-layout>
</template>

<script>
import GroupSelect from '../components/GroupSelect'
export default {
  components: {
    GroupSelect
  },
  data() {
    return {
      test: []
    }
  },
  mounted() {
    this.$axios.get('http://localhost:8080/pages').then((response) => {
      this.test = response.data
    })
  },
  methods: {
    getUrl(id) {
      const url = `https://vk.com/id${id}`
      return url
    },
    loadMembers() {}
  }
}
</script>
