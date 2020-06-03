<template>
  <div class="home">
    <loading v-if="loading"/>
      <div v-if="isAuthenticated">
        <h1>Добро пожаловать в личный кабинет!</h1>
      </div>
  <div v-if="!isAuthenticated && authStatus !== 'loading'">
    <h1>Приложение "Личный кабинет"</h1>
     <login/>
  </div>
  </div>
</template>

<script>
  
  import { mapGetters } from 'vuex'
  import Login from 'components/login'
  import Account from 'components/account'
  export default {
    components: {
      Login
    },
    name: 'home',
    computed: {
      ...mapGetters(['isAuthenticated', 'authStatus']),
      loading: function () {
        return this.authStatus === 'loading' && !this.isAuthenticated
      },
    },
  }
</script>

<style scoped>
.home {
    display: flex;
    align-items: center;
    flex-direction: column;
    
  }
</style>
