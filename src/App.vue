<template>
  <header>
    <Logo />
  </header>
  <main>
    <router-view/>
  </main>
  <nav id="nav">
    <router-link :to="{ name: 'Dashboard' }">Dashboard</router-link> |
    <router-link :to="{ name: 'Details' }">Details</router-link> |
    <router-link :to="{ name: 'Calculator' }">Calculator</router-link> |
    <router-link :to="{ name: 'Diagram' }">Diagram</router-link>
    <!-- <router-link :to="{ name: 'User' }" username="lorem">User</router-link> -->
  </nav>
</template>

<script>
import Logo from '@/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
