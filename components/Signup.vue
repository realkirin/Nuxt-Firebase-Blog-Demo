<template>
  <v-container>
    <v-layout row v-if="error">
      <app-alert @dismissed="onDismissed" :text="error.message"></app-alert>
    </v-layout>
    <v-layout row>
      <v-card>
        <v-card-text>
          <v-container>
            <form @submit.prevent="onSignup">
              <v-layout row>
                <v-text-field name="email" label="Mail" id="email" v-model="email" type="email" required></v-text-field>
              </v-layout>
              <v-layout row>
                <v-text-field name="password" label="Password" id="password" v-model="password" type="password" required></v-text-field>
              </v-layout>
              <v-layout row>
                <v-text-field name="confirmPassword" label="Confirm Password" id="confirmPassword" v-model="confirmPassword" type="password"
                  :rules="[comparePasswords]"></v-text-field>
              </v-layout>
              <v-layout row>
                <div class="text-xs-center">
                  <v-btn round type="submit" :disabled="loading" :loading="loading">
                    Sign up
                    <v-icon right>lock_open</v-icon>
                    <span slot="loader" class="custom-loader">
                      <v-icon light>cached</v-icon>
                    </span>
                  </v-btn>
                </div>
                <div class="text-xs-center">
                  <v-btn round class="red" dark :disabled="loading" :loading="loading" @click.prevent="onSigninGoogle">Login with Google
                    <v-icon right dark>lock_open</v-icon>
                    <span slot="loader" class="custom-loader">
                      <v-icon light>cached</v-icon>
                    </span>
                  </v-btn>
                </div>
              </v-layout>
            </form>
          </v-container>
        </v-card-text>
      </v-card>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        email: '',
        password: '',
        confirmPassword: ''
      }
    },
    computed: {
      comparePasswords() {
        return this.password !== this.confirmPassword ? 'Passwords do not match' : ''
      },
      user() {
        return this.$store.getters.user
      },
      error() {
        return this.$store.getters.error
      },
      loading() {
        return this.$store.getters.loading
      }
    },
    methods: {
      onSignup() {
        this.$store.dispatch('signUserUp', {
          email: this.email,
          password: this.password
        })
      },
      onSigninGoogle() {
        this.$store.dispatch('signUserInGoogle')
      },
      onDismissed() {
        this.$store.dispatch('clearError')
      }
    }
  }

</script>
