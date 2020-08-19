<template>
  <v-layout column justify-center align-center>
    <v-row>
      <group-select ref="groupSelect" />
      <v-btn :loading="loading" @click="loadMembers"
        >Загрузить пользователей</v-btn
      >
    </v-row>
    <v-row>
      <v-pagination
        v-model="page"
        :length="pagesCount"
        :total-visible="totalVisible"
      />
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
    <v-row>
      <v-pagination
        v-model="page"
        :length="pagesCount"
        :total-visible="totalVisible"
      />
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
      test: [],
      loading: false,
      page: 1,
      pagesCount: 0,
      totalVisible: 7,
      offset: 0,
      perPage: 20,
      totalCount: 0
    }
  },
  watch: {
    page(newVal) {
      this.offset = this.offset + this.perPage
      this.offset = this.page * this.perPage
      this.getPages(this.perPage, this.offset)
    }
  },
  mounted() {
    this.getPages(this.perPage, this.offset)
  },
  methods: {
    getUrl(id) {
      const url = `https://vk.com/id${id}`
      return url
    },
    async loadMembers() {
      this.loading = true
      await this.$refs.groupSelect.loadMembers()
      this.loading = false
    },
    getPages(perPage, offset) {
      const params = { perPage, offset }
      this.$axios
        .$get('http://localhost:8080/pages', { params })
        .then((response) => {
          this.test = response.items.filter((d) => d)
          this.totalCount = response.totalCount
          this.pagesCount = Math.ceil(this.totalCount / this.perPage)
          console.log('RESPONSE', response)
        })
    }
  }
}
</script>
