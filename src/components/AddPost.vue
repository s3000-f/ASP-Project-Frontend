<template>
  <v-dialog max-width="450px" v-model="addPostDialog">
    <!--<v-btn slot="activator" color="primary" dark>Open Dialog</v-btn>-->
    <v-card ma-5>
      <v-toolbar dark color="primary darken-1">
        <v-toolbar-title>
          <span>New Post</span>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items>
          <!--<v-btn dark flat @click="alert()">ذخیره</v-btn>-->
          <v-btn flat icon dark @click="addPostDialog = false">
            <v-icon>clear</v-icon>
          </v-btn>
        </v-toolbar-items>

      </v-toolbar>
      <v-layout row wrap mt-3 ml-3 mr-3 >
        <v-flex xs12>
            <v-text-field
              name="input-7-1"
              label="Image Address"
              v-model="fileName"
            ></v-text-field>
            <br>
            <v-textarea
              name="input-7-1"
              label="Caption"
              v-model="caption"
            ></v-textarea>
            <v-btn
              color="success"
              @click="validate"
            >
              Add
            </v-btn>

            <v-btn
              color="error"
              @click="reset"
            >
              Reset
            </v-btn>
        </v-flex>
      </v-layout>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from 'axios'

export default {
  name: 'AddPost',
  props: ['addPostDialog'],
  data () {
    return {
      fileName: '',
      caption: ''
    }
  },
  methods: {
    validate () {
      if (this.fileName !== '') {
        let data = {
          FileName: this.fileName,
          Caption: this.caption,
          UserId: this.$store.state.user.id
        }
        axios.post('http://localhost:8181/api/posts', data).then(response => {
          console.log(response)
        }).catch(e => console.log(e))
      }
    },
    reset () {
      this.caption = ''
      this.fileName = ''
    }
  }
}
</script>

<style scoped>

</style>
