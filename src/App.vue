<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
    </div> -->
    <transition name="fade" mode="out-in">
      <router-view/>
    </transition>
    <svg style="height: 0;">
      <defs>
        <filter id="fe-dropshadow" x="-2" y="-2" width="100" height="100">
          <feDropShadow  dx="0" dy="0" stdDeviation="1.5" />
        </filter>
      </defs>
    </svg>
  </div>
</template>

<style lang="scss">
@import '@/assets/animations/index.scss';

body {
  background-color: #333;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #eee;
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
