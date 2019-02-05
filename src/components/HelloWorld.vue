<template>
  <v-container>
    <Comments
      :postID="showingID"
      :post="posts[showingID]"
      userID="10"
      :showComments="showComments"
    >
    </Comments>
    <add-post :add-post-dialog="addPostDialog"></add-post>

    <v-layout v-for="item in posts" :key="item.id" mb-3>
      <v-flex xs12>
        <v-card>
          <v-img
            :src="item.fileName"
            aspect-ratio="2.75"
          ></v-img>

          <v-card-title primary-title>
            <div>
              <h4 class="headline mb-0">{{item.userName}}
                <v-btn icon @click="like(item)">
                  <v-icon v-if="item.likes.includes(uid)" color="red">favorite</v-icon>
                  <v-icon v-else>favorite_border</v-icon>
                </v-btn>
                <span class="font-weight-light">
                {{item.likes.length}} Likes
                </span>
              </h4>
              <div>{{item.caption}}</div>
            </div>
          </v-card-title>

          <v-card-actions>
            <v-btn flat color="orange" @click="showCommentsDialog(posts.indexOf(item))">Comments</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
    <v-btn
      fab
      small
      color="red lighten-4"
      top
      right
      absolute
      @click="addPostDialog = true"
    >
      <v-icon>add</v-icon>
    </v-btn>
  </v-container>
</template>

<script>
import Comments from './Comments'
import AddPost from './AddPost'
import axios from 'axios'

export default {
  components: { AddPost, Comments },
  computed: {
    uid: function () {
      return this.$store.state.user.id
    }
  },
  data: () => ({
    showComments: false,
    addPostDialog: false,
    showingID: 1,
    posts: [
      {
        id: 1,
        userName: 'mammad',
        fileName: 'https://cdn.vuetifyjs.com/images/cards/desert.jpg',
        caption: 'sample caption',
        likes: [10, 20, 30]
      },
      {
        id: 2,
        userName: 'hasan',
        fileName: 'https://cdn.vuetifyjs.com/images/cards/desert.jpg',
        caption: 'sample caption asdkfjasdl;fj kafl ja;ldkg jal;dfg ',
        likes: []
      }
    ]
  }),
  methods: {
    like (item) {
      if (item.likes.includes(this.uid)) {
        // unlike
        axios.delete(`http://localhost:8181/api/posts/${item.id}/like/${this.uid}`).then(response => {
          console.log(response)
          item.likes = item.likes.filter((value, index, arr) => {
            return value !== this.uid
          })
        }).catch(e => console.log(e))
      } else {
        let data = {
          UserId: this.uid,
          PostId: item.id
        }
        axios.post(`http://localhost:8181/api/posts/${item.id}/like`, data).then(response => {
          console.log(response)
          item.likes.push(this.uid)
        }).catch(e => console.log(e))
      }
    },
    showCommentsDialog (id) {
      this.showingID = id
      console.log(this.showingID)
      this.showComments = true
    },
    getPosts () {
      axios.get('http://localhost:8181/api/posts').then(response => {
        console.log(response)
        this.posts = response.data
      }).catch(e => console.log(e))
    }
  },
  created () {
    this.getPosts()
  }
}
</script>

<style>

</style>
