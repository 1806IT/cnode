<template>
  <div id="app">
    <Header></Header>
    <div class="main">
      <router-view name="main"></router-view>
      <router-view name="SlideBar"></router-view>
    </div>

  </div>
</template>

<script>
  import Header from './components/Header'
  import PostList from './components/PostList'

export default {
  name: 'App',
  components:{
    Header,
    PostList
  }
}
</script>

<style>
.main{
  width: 100%;
  margin: 0 auto;
  display: flex;

}
</style>
