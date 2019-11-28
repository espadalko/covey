<template>
  <div id="app">
    <Test1/>
  </div>
</template>

<script>
import Test1 from '@/components/Test1.vue'
export default {
  name: 'app',
  components: {
    Test1
  }
}
</script>

<style lang="stylus">
*
  box-sizing border-box
body
  background-color #ccc
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #2c3e50
</style>
