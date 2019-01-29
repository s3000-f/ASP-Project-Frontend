<template>
  <v-app color = "lightgray">
    <v-toolbar app>
      <v-toolbar-title class="headline" to="/home">

          <v-btn flat to="/">
            <v-img
              :src="`https://www.instagram.com/static/images/ico/xxhdpi_launcher.png/99cf3909d459.png`"
              aspect-ratio="1"
              contain
              height="24"
            ></v-img>
          </v-btn>
            <span class="font-italic font-weight-light" to="/home">Instagram &nbsp;</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn
          flat
          to="users"
        >
          <span class="mr-2">All Users</span>
        </v-btn>
        <v-btn
          flat
          to="/about"
        >
          <span class="mr-2">About</span>
        </v-btn>

      </v-toolbar-items>
    </v-toolbar>

    <v-content>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      //
    }
  }
}
</script>
