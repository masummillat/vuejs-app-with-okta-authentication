<template>
  <div id="app">
    <b-navbar toggleable="md" type="dark" variant="dark">
      <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
      <b-navbar-brand to="/">My Vue App</b-navbar-brand>
      <b-collapse is-nav id="nav_collapse">
        <b-navbar-nav>
          <router-link to="/">Home</router-link>
          <router-link to="/posts-manager">Posts Manager</router-link>
          <button href="#" @click.prevent="login" v-if="!activeUser">Login</button>
          <button href="#" @click.prevent="logout" v-else>Logout</button>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <!-- routes will be rendered here -->
    <router-view/>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data(){
      return {
        activeUser: null,
      }
    },
    async created (){
      await this.refreshActiveUser()
    },
    watch: {
      '$route': 'refreshActiveUser'
    },
    methods: {
      login(){
        this.$auth.loginRedirect();
      },
      async refreshActiveUser(){
        this.activeUser = await this.$auth.getUser();
      },
      async logout(){
          await this.$auth.logout();
          await this.refreshActiveUser();
          this.$router.push('/')
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
