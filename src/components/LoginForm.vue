<template>
  <v-container grid-list-md text-xs-center>
    <v-layout row wrap>
      <v-flex xs10 sm6 offset-xs1 offset-sm3>
        <v-form
          class="login-form"
          ref="form"
          v-model="valid"
          lazy-validation
          @keydown.enter.native="login"
        >
          <v-text-field v-model="email" :rules="emailRules" label="E-mail" required ref="myEmail"></v-text-field>

          <v-text-field v-model="password" :rules="passwordRules" label="Password" required></v-text-field>

          <v-btn :disabled="!valid" color="success" @click="login">Login</v-btn>
        </v-form>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: "LoginForm",
  data: () => ({
    authenticated: false,
    valid: true,
    password: "",
    passwordRules: [
      v => !!v || "Password is required",
      v => (v && v.length <= 10) || "Password must be less than 10 characters"
    ],
    email: "",
    emailRules: [
      v => !!v || "E-mail is required",
      v => /.+@.+/.test(v) || "E-mail must be valid"
    ]
  }),
  created() {
    if (this.$auth.currentUser) {
      this.authenticated = true;
      this.$store.commit("setAuthenticated", true);
    }
  },
  mounted() {
    if (this.authenticated) {
      this.$router.replace({ name: "inventory" });
    }
  },
  methods: {
    login() {
      this.$auth
        .signInWithEmailAndPassword(this.email, this.password)
        .then(() => {
          this.$store.commit("setAuthenticated", true);
          this.authenticated = true;
          this.$router.push({ name: "inventory" });
        })
        .catch(e => console.log(e.message));
    }
  }
};
</script>
<style lang="scss">
.login-form {
  .v-text-field .v-input__control > .v-input__slot:before {
    border-color: #814923;
  }
  .v-label {
    color: #814923;
  }
  .v-input:not(.v-input--is-disabled) input,
  .theme--dark.v-input:not(.v-input--is-disabled) textarea {
    color: #814923;
  }
}
</style>


