<template>
  <div id="app">
    <HeaderNavigation/>
    <AffiliateLinks/>
  </div>
</template>

<script>
import AffiliateLinks from './components/AffiliateLinks.vue'
import HeaderNavigation from './components/HeaderNavigation.vue'
export default {
  name: 'App',
  components: {
    AffiliateLinks,
    HeaderNavigation
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: auto;
  max-width: 95vw;
  @media only screen and (min-width: 991px) {
    max-width: 80vw;
  }
}
</style>
