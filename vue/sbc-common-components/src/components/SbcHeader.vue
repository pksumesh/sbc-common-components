<template>
  <header class="app-header" fixed app>
    <div class="container">
      <a href="/" class="navbar-brand">
        <img class="brand-img" src="../assets/img/gov3_bc_logo.png"
          alt="Province of British Columbia Logo"
          title="Province of British Columbia"/>
      </a>
      <span class="brand-title">BC Registries & Online Services</span>
      <div class="app-header__actions">
        <span v-if="authorized"><button class="v-btn v-btn--outline v-btn--depressed theme--light" @click="logout">Sign Out</button></span>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import Vue from 'vue'
import AuthService from '../services/auth.services'

export default Vue.extend({
  name: 'sbc-header',
  props: {
    authURL: String
  },
  computed: {
    authorized ():boolean {
      let auth = sessionStorage.getItem('KEYCLOAK_TOKEN')
      return !!auth
    }
  },
  methods: {
    logout () {
      AuthService.logout(sessionStorage.getItem('KEYCLOAK_REFRESH_TOKEN'), this.authURL).then(response => {
        if (response.status === 204) {
          sessionStorage.removeItem('KEYCLOAK_REFRESH_TOKEN')
          sessionStorage.removeItem('KEYCLOAK_TOKEN')
          sessionStorage.removeItem('REGISTRIES_TRACE_ID')
          sessionStorage.removeItem('AUTH_WEB')
          window.location.assign('/auth')
        } else {
          console.log('Logout failed. ' + response)
        }
      }).catch((error: any) => {
        console.log('fetchError' + error)
      })
    }
  }
})
</script>

<style lang="stylus" scoped>
  @import "../assets/styl/theme.styl"

  .app-header
    position fixed
    width 100%
    color #fff
    border-bottom 3px solid $BCgovGold5
    background-color $BCgovBlue5

    .container
      display flex
      align-items center
      padding-top 0
      padding-bottom 0

  .app-header__actions
      margin-left auto

    .v-btn
      margin-right 0
      color #ffffff
      border-color #ffffff

  .brand-img
    margin-top 0.3rem
    margin-right 1.25rem

  .brand-title
    margin-top 0.2rem
    font-size 1.125rem
    font-weight 300
</style>
