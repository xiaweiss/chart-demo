<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/g2">G2</router-link> |
      <router-link to="/f2">F2</router-link> |
      <router-link to="/echarts">Echarts</router-link> |
      <router-link to="/tooltip">ToolTip</router-link> |
      <router-link to="/test">Test</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="stylus">
@import '../node_modules/normalize.css/normalize.css'
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>
