<template>
  <v-container fluid>
    <h4>Login</h4>
    <form>
      <v-row>
        <v-text-field
          label="Email"
          hide-details="auto"
          v-model="email"
        ></v-text-field>
      </v-row>
      <v-row>
        <v-text-field
          label="Password"
          hide-details="auto"
          v-model="password"
        ></v-text-field>
      </v-row>
      <v-row>
        <v-btn
          @click="handleSubmit"
          block
        >
          Login
        </v-btn>
      </v-row>
    </form>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    handleSubmit (e) {
      e.preventDefault()
      if (this.password.length > 0) {
        this.$http
          .post('http://localhost:3000/login', {
            email: this.email,
            password: this.password
          })
          .then((response) => {
            localStorage.setItem('user', JSON.stringify(response.data.user))
            localStorage.setItem('jwt', response.data.token)

            if (localStorage.getItem('jwt') != null) {
              this.$emit('loggedIn')
              if (this.$route.params.nextUrl != null) {
                this.$router.push(this.$route.params.nextUrl)
              } else {
                this.$router.push('dashboard')
              }
            }
          })
          .catch(function (error) {
            console.error(error.response)
          })
      }
    }
  }
}
</script>
