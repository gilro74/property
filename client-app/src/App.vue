<template>
  <div id="app">
    <b-navbar toggleable="md" type="dark" variant="dark">
      <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
      <b-navbar-brand to="/">Property App</b-navbar-brand>
      <b-collapse is-nav id="nav_collapse">
        <b-navbar-nav>
          
          <b-nav-item href="#" @click.prevent="login" v-if="!activeUser">Login</b-nav-item>
          <b-nav-item href="#" @click.prevent="logout" v-else>Logout</b-nav-item>
          <b-nav-item to="/">Home</b-nav-item>
          <b-nav-item to="/user-manager">User Management</b-nav-item>
          <b-nav-item to="/issue-manager">Issue Management</b-nav-item>
          <b-nav-item to="/fines">Fines</b-nav-item>
          <b-nav-item to="/notices">Notices</b-nav-item>
          <b-nav-item to="/reports">Reports</b-nav-item>

        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <!-- routes will be rendered here -->
    <router-view />
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      activeUser: null
    }
  },
  async created () {
    await this.refreshActiveUser()
  },
  watch: {
    // everytime a route is changed refresh the activeUser
    '$route': 'refreshActiveUser'
  },
  methods: {
    async login () {
      this.$auth.signInWithRedirect()
    },
    async refreshActiveUser () {
      if (this.authState.isAuthenticated) {
        this.activeUser = await this.$auth.getUser()
      }
    },
    async logout () {
      await this.$auth.signOut()
      await this.refreshActiveUser()
      this.$router.push('/')
    }
  }
}
</script>