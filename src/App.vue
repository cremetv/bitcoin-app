<template>
  <header id="header">
    <Logo />
  </header>
  <main id="main">
    <router-view/>
  </main>
  <nav id="nav">
    <router-link :to="{ name: 'Dashboard' }">
      <span class="icon">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M3 13H11V3H3V13ZM3 21H11V15H3V21ZM13 21H21V11H13V21ZM13 3V9H21V3H13Z" fill="currentColor"/>
        </svg>
      </span>
      <span>Dashboard</span>
    </router-link>
    <router-link :to="{ name: 'Details' }">
      <span class="icon">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M14.8 5L14.8 8L5 8L5 5L14.8 5ZM19 10.6L19 13.4L5 13.4L5 10.6L19 10.6ZM11 16.2L11 19L5 19L5 16.2L11 16.2Z" fill="currentColor"/>
        </svg>
      </span>
      <span>Details</span>
    </router-link>
    <router-link :to="{ name: 'Calculator' }">
      <span class="icon">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M19 3H5C3.9 3 3 3.9 3 5V19C3 20.1 3.9 21 5 21H19C20.1 21 21 20.1 21 19V5C21 3.9 20.1 3 19 3ZM13.03 7.06L14.09 6L15.5 7.41L16.91 6L17.97 7.06L16.56 8.47L17.97 9.88L16.91 10.94L15.5 9.54L14.09 10.95L13.03 9.89L14.44 8.48L13.03 7.06ZM6.25 7.72H11.25V9.22H6.25V7.72ZM11.5 16H9.5V18H8V16H6V14.5H8V12.5H9.5V14.5H11.5V16ZM18 17.25H13V15.75H18V17.25ZM18 14.75H13V13.25H18V14.75Z" fill="currentColor"/>
        </svg>
      </span>
      <span>Calculator</span>
    </router-link>
    <router-link :to="{ name: 'Diagram' }">
      <span class="icon">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M3.5 18.49L9.5 12.48L13.5 16.48L22 6.92001L20.59 5.51001L13.5 13.48L9.5 9.48001L2 16.99L3.5 18.49Z" fill="currentColor"/>
        </svg>
      </span>
      <span>Diagram</span>
    </router-link>
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
@import '_styles/_variables.less';
@import '_styles/_mixins.less';
@import '_styles/_base.less';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  flex: 1;

  display: flex;
  flex-direction: column;
}

#header {
  background: var(--background-secondary);
  position: sticky;
  top: 0;
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

#main {
  flex: 1;
  background: var(--background-primary);
  // padding: 1rem;

  h1 {
    margin-bottom: 1rem;
  }
}

#nav {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;

  position: sticky;
  bottom: 0;

  padding: .5rem;
  background: var(--background-secondary);
  background: var(--background-primary);

  a {
    display: flex;
    justify-content: center;
    align-items: center;
    flex: 1;
    padding: 2rem .5rem;
    background: var(--background-secondary);
    border-radius: 1rem;
    .fw(--fw-bold);
    color: var(--text-normal);
    text-decoration: none;

    + a {
      margin-left: .5rem;
    }

    &.router-link-exact-active {
      color: var(--focus-primary);
      background: var(--background-tertiary);
    }
  }

  span:not(.icon) {
    display: none;
  }

  .icon {
    // margin-right: .5rem;
    position: absolute;

    svg {
      vertical-align: middle
    }
  }
}
</style>
