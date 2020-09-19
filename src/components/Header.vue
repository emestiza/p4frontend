<template>
  <b-navbar>
    <template slot="brand">
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        <span class="logo">StudyDB</span>
      </b-navbar-item>
    </template>

    <template slot="start">
      <b-navbar-item href="#">
        <router-link to="/main">Main</router-link>
      </b-navbar-item>
      <b-navbar-item href="#">
        <router-link to="/subjects">Subjects</router-link>
      </b-navbar-item>
      <b-navbar-item href="#">
        <router-link to="/topics">Topics</router-link>
      </b-navbar-item>
    </template>

    <template slot="end">
      <b-navbar-item tag="div">
        <div class="buttons">
          <router-link
            :to="{ name: 'Signup', query: { URL: this.URL }}"
            v-bind:URL="URL"
            v-if="!loggedIn"
          >
            <button class="button is-link">Sign up</button>
          </router-link>

          <router-link
            :to="{ name: 'Login', query: { URL: this.URL }}"
            v-bind:URL="URL"
            v-if="!loggedIn"
          >
            <button class="button is-link">Log in</button>
          </router-link>
          <button v-if="loggedIn" class="button is-link" @click="logout">Logout</button>
        </div>
      </b-navbar-item>
    </template>
  </b-navbar>
</template>

<script>
export default {
  name: "Header",
  props: ["URL", "loggedIn"],
  methods: {
    logout: function() {
      this.$emit("logout");
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap");

nav {
  width: 80%;
  margin: 10px auto;
}

span.logo {
  font-family: Noto Sans;
  font-size: 2em;
}
</style>