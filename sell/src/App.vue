<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
      <div class="tab-item">商品</div>
      <div class="tab-item">评论</div>
      <div class="tab-item">商家</div>
    </div>
    <div class="content">
      I'm content
      <router-view/>
    </div>
  </div>
</template>

<script>
import header from './components/header/Header'
export default {
  name: 'App',
  components: {
    'v-header': header
  }
}
</script>

<style lang="stylus">
  #app
    .tab
      display flex
      width 100%
      height 2.5rem
      line-height 2.5rem
      .tab-item
        flex 1
        text-align center
</style>
