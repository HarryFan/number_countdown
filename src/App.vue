<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <NumberCountDown :time="10" :sound=false />
    <NumberCountDownWithProgressBar :time="10" :sound=true />
    <NumberCountDownWithImages :time="10" :sound="false" />
  </div>
</template>

<script>
  import NumberCountDown from './components/NumberCountDown.vue';
  import NumberCountDownWithImages from './components/NumberCountDownWithImages.vue';
import NumberCountDownWithProgressBar from './components/NumberCountDownWithProgressBar.vue';
  export default {
    name: 'App',
    components: {
    NumberCountDown,
    NumberCountDownWithImages,
    NumberCountDownWithProgressBar
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

  .img {
    width: auto;
  }
</style>