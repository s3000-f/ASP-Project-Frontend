<template>
  <v-container>
    <v-layout>
      <v-flex xs2></v-flex>
      <v-flex xs8>

        <v-list two-line>
          <template v-for="item in users"
          >
            <v-list-tile
              :key="item.name"
              avatar
              transition="fade-transition"
            >
              <v-list-tile-avatar>
                <v-icon class="grey lighten-1 white--text">person</v-icon>
              </v-list-tile-avatar>

              <v-list-tile-content>
                <v-list-tile-title>{{ item.userName }}</v-list-tile-title>
              </v-list-tile-content>

              <v-list-tile-action>
                <v-btn icon ripple>
                  <v-icon color="red lighten-1">delete_forever</v-icon>
                </v-btn>

              </v-list-tile-action>
            </v-list-tile>
            <v-divider :key="item.id"
            ></v-divider>
          </template>

        </v-list>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Users',
  data () {
    return {
      users: [
        {
          userName: 'mammad',
          id: 1
        }
      ]
    }
  },
  created () {
    this.getUsers()
  },
  methods: {
    getUsers () {
      axios.get('http://localhost:8181/api/users')
        .then(response => {
          console.log(response)
          this.users = response.data
        }).catch(e => console.log(e))
    }
  }
}
</script>

<style scoped>

</style>
