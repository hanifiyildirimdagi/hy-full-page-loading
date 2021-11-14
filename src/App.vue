<template>
  <div id="app">
    <hy-full-page-loading :loading="loading" />
  </div>
</template>

<script>
import HyFullPageLoading from "./components/hy-full-page-loading.vue";
export default {
  name: 'App',
  components: {
    HyFullPageLoading
  },
  data : () => ({
loading:false
  }),
  mounted(){
    let that = this;
    setTimeout(function(){
      that.loading = true;
      setTimeout(function(){
        that.loading = false;
      },2500);
    },2500);
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
  margin-top: 60px;
}
</style>
