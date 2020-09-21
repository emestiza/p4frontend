<template>
  <div class="signup">
    <b-field label="Firstname" type="is-success">
      <b-input v-model="first_name" placeholder="first name"></b-input>
    </b-field>

    <b-field label="Lastname" type="is-success">
      <b-input v-model="last_name" placeholder="last name"></b-input>
    </b-field>

    <b-field label="Username" type="is-success">
      <b-input v-model="username" placeholder="unique username"></b-input>
    </b-field>

    <b-field label="Email" type="is-success">
      <b-input v-model="email" placeholder="valid email"></b-input>
    </b-field>

    <b-field label="Password">
      <b-input
        type="password"
        v-model="password"
        password-reveal
        placeholder="password requires 8 characters"
      ></b-input>
    </b-field>
    <br />
    <button class="button is-danger" @click="handleSignup">Sign Up</button>
  </div>
</template>

<script>
export default {
  name: "Signup",
  data: function() {
    return {
      first_name: "",
      last_name: "",
      username: "",
      email: "",
      password: ""
    };
  },
  methods: {
    handleSignup: function() {
      console.log(this.$route.query.URL);
      fetch(`${this.$route.query.URL}/auth/users/register/`, {
        method: "post",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          first_name: this.first_name,
          last_name: this.last_name,
          username: this.username,
          email: this.email,
          password: this.password
        })
      }).then(response => {
        if (response.status == 201) {
          response.json().then(data => {
            console.log(data);
            this.$router.push({
              name: "Login",
              query: this.$route.query,
              params: { message: "Sign up complete! Log in to continue." }
            });
          });
        } else {
          return response.json();
        }
      });
    }
  }
};
</script>

<style>
.signup {
  width: 25%;
  margin: 10px auto;
}
</style>
