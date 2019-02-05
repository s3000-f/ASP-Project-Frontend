<template>
  <v-container>
    <v-layout justify-center mb-5>
      <v-flex class="font-weight-light font-italic text-xs-center display-2"><span>Instagram Login</span></v-flex>
    </v-layout>
    <v-layout ma-5></v-layout>
    <v-layout justify-center align-center>

      <v-flex md6 xs12 justify-center>
        <v-card>
          <v-card-text>
            <v-form v-if="logreg" v-model="login.valid" ref="loginForm" @submit.prevent="loginSubmit">
              <v-flex xs12>
                <v-text-field
                  v-model="login.mobile"
                  :rules="[validationRules.required, validationRules.email]"
                  label="Username"
                  autocomplete="username"
                  name="email"
                ></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                  v-model="login.password"
                  :append-icon="login.showPassword ? 'visibility_off' : 'visibility'"
                  :rules="[validationRules.required]"
                  :type="login.showPassword ? 'text' : 'password'"
                  label="Password"
                  @click:append="login.showPassword = !login.showPassword"
                  autocompelete="current-password"
                  name="password"
                ></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-btn block color="primary" type="submit" :loading="login.loading">Login</v-btn>
                <v-btn block outline color="secondary" @click="logreg = false">Register</v-btn>
              </v-flex>
            </v-form>
            <v-form v-else v-model="login.valid" ref="registerForm" @submit.prevent="registerSubmit">
              <v-flex xs12>
                <v-text-field
                  v-model="login.mobile"
                  :rules="[validationRules.required, validationRules.email]"
                  label="Username"
                  autocomplete="username"
                  name="email"
                ></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                  v-model="login.password"
                  :append-icon="login.showPassword ? 'visibility_off' : 'visibility'"
                  :rules="[validationRules.required]"
                  :type="login.showPassword ? 'text' : 'password'"
                  label="Password"
                  @click:append="login.showPassword = !login.showPassword"
                  autocompelete="current-password"
                  name="password"
                ></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-btn block color="primary" type="submit" :loading="login.loading">Register</v-btn>
                <v-btn block outline color="secondary" @click="logreg = true">Login</v-btn>

              </v-flex>
            </v-form>
          </v-card-text>

        </v-card>

      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'
import { mapActions } from 'vuex'
export default {
  name: 'Login',
  data () {
    return {
      logreg: true,
      login: {
        valid: false,
        mobile: '',
        password: '',
        showPassword: false,
        loading: false
      },
      validationRules: {
        required: value => !!value || 'Field Cannot Be Empty',
        email: v => /[a-z0-9]+(?:\.[a-z0-9]+)*/.test(v) || 'Wrong Format'
      }
    }
  },
  methods: {
    ...mapActions([
      'setUser'
    ]),
    loginSubmit () {
      if (this.$refs.loginForm.validate()) {
        this.login.loading = true
        let data = new FormData()
        data.set('userName', this.login.mobile)
        data.set('password', this.login.password)
        axios.post('http://localhost:8181/api/login', data, { headers: { 'Content-Type': 'multipart/form-data' } }).then(response => {
          console.log(response)
          this.setUser(response.data)
          this.$router.push('home')
        }).catch(e => alert('Wrong Username or Password'))
        this.login = {
          valid: false,
          mobile: '',
          password: '',
          showPassword: false,
          loading: false
        }
      }
    },
    registerSubmit () {
      if (this.$refs.loginForm.validate()) {
        this.login.loading = true
        let data = {
          username: this.login.mobile,
          password: this.login.password
        }
        axios.post('http://localhost:8181/api/users', data).then(response => {
          console.log(response)
          this.setUser(response.data)
          this.$router.push('home')
        }).catch(e => alert('User Already Exists'))
        // TODO APi call to Login Controller and set user id in store
        this.$router.push('home')
        this.login = {
          valid: false,
          mobile: '',
          password: '',
          showPassword: false,
          loading: false
        }
      }
    }

  },
  created () {
    if (this.$store.state.user != null) {
      this.$router.push('home')
    }
  }
}
</script>

<style scoped>

</style>
