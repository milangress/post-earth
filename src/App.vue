<template>
  <div id="app">
    <div id="nav">
      <!--<router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>-->
    </div>
    <router-view/>
  </div>
</template>

<style lang="stylus">
  *
    box-sizing: border-box
  body
    //background: white
    //background: linear-gradient(-45deg, rgba(0,0,0,0.22), rgba(255,255,255,0.25))
    min-height 100vh
    cursor:  url('./assets/coin.png') 60 60, auto
    margin 0
  #app
    font-family "Arial Narrow", Arial
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    height 100%
    //margin-top 60px
    //background white
</style>
