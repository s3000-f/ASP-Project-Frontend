<template>
  <v-dialog max-width="800px" v-model="showComments">
    <!--<v-btn slot="activator" color="primary" dark>Open Dialog</v-btn>-->
    <v-card ma-5>
      <v-toolbar dark color="primary darken-1">
        <v-toolbar-title>
          <span>Comments</span>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items>
          <!--<v-btn dark flat @click="alert()">ذخیره</v-btn>-->
          <v-btn flat icon dark @click="showComments = false">
            <v-icon>clear</v-icon>
          </v-btn>
        </v-toolbar-items>

      </v-toolbar>
      <v-layout row wrap v-if="postID !== -1" mt-3 ml-3>
        <v-flex md4 v-for="item in ps.comments" :key="item.id">
          <span class="font-weight-bold">{{item.username}}: &nbsp;</span>
          <span class="font-weight-light">{{item.data}}</span>
        </v-flex>
      </v-layout>
      <br>
      <br>
      <v-layout>
        <v-flex xs4></v-flex>
        <v-flex xs3>
          <v-text-field
            label="Add Comment"
            v-model="newComment"
          ></v-text-field>
        </v-flex>
        <v-flex xs1>
          <v-btn flat icon @click="addComment">
            <v-icon>send</v-icon>

          </v-btn>
        </v-flex>
        <v-flex xs4></v-flex>
      </v-layout>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Comments',
  props: ['post', 'postID', 'userID', 'showComments'],
  data () {
    return {
      ps: this.post,
      newComment: ''
    }
  },
  methods: {
    addComment () {
      let cm = {
        UserId: this.$store.state.user.id,
        PostId: this.post.id,
        data: this.newComment
      }
      axios.post(`http://localhost:8181/api/posts/${this.post.id}/comments`, cm).then(response => {
        this.ps.comments.push(response.data)
        console.log(response)
      }).catch(e => console.log(e))
      this.newComment = ''
    },
    getComments () {
      axios.get(`http://localhost:8181/api/posts/${this.post.id}/comments`).then(response => {
        this.ps.comments = response.data
      }).catch(e => console.log(e))
    }
  },
  created () {
    this.getComments()
  }
}
</script>

<style scoped>

</style>
