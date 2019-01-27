<template>
  <v-container>
    <v-dialog v-model="showComments">
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
        <v-layout row wrap v-if="showingID !== -1" mt-3 ml-3>
          <v-flex md4 v-for="item in posts[showingID].comments" :key="item.id">
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

    <v-layout v-for="item in posts" :key="item.id" mb-3>
      <v-flex xs12>
        <v-card>
          <v-img
            :src="item.image"
            aspect-ratio="2.75"
          ></v-img>

          <v-card-title primary-title>
            <div>
              <h4 class="headline mb-0">{{item.username}}
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
  </v-container>
</template>

<script>
export default {
  data: () => ({
    uid: 1,
    showComments: false,
    showingID: 1,
    newComment: '',
    posts: [
      {
        id: 1,
        username: 'mammad',
        image: 'https://cdn.vuetifyjs.com/images/cards/desert.jpg',
        caption: 'sample caption',
        likes: [10, 20, 30],
        comments: [
          {
            id: 0,
            username: 'soosk',
            data: 'jkadhvkja lkajv hdklj hadkjg adlkgblfk d'
          }
        ]
      },
      {
        id: 2,
        username: 'hasan',
        image: 'https://cdn.vuetifyjs.com/images/cards/desert.jpg',
        caption: 'sample caption asdkfjasdl;fj kafl ja;ldkg jal;dfg ',
        likes: [],
        comments: []
      }
    ]
  }),
  methods: {
    like (item) {
      if (item.likes.includes(this.uid)) {
        // unlike
        item.likes = item.likes.filter((value, index, arr) => {
          return value !== this.uid
        })
      } else {
        item.likes.push(this.uid)
      }
    },
    showCommentsDialog (id) {
      this.showingID = id
      console.log(this.showingID)
      this.showComments = true
    },
    addComment () {
      let cm = {
        id: 10,
        username: 'mam',
        data: this.newComment
      }
      this.posts[this.showingID].comments.push(cm)
      this.newComment = ''
    }
  }
}
</script>

<style>

</style>
