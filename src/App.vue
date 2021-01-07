<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Pomodoro</router-link> | 
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f9fcff;
}













#nav {
  padding: 30px;
  text-align: left;
  a {
    font-weight: bold;
    color: rgb(211, 194, 43);

    &.router-link-exact-active {
      color: rgb(236, 215, 24);
    }
  }
}
</style>
