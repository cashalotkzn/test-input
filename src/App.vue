<template>
  <div id="app">
    <IndexPage/>
  </div>
</template>

<script>
import IndexPage from './components/IndexPage.vue';

export default {
  name: 'App',
  components: {
    IndexPage,
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@600&display=swap&weight');
@import "assets/scss/colors";

body, html {
  padding: 0;
  margin: 0;
  height: 100%;
  width: 100%;

  * {
    font-family: 'Open Sans', sans-serif;
  }
}

#app {
  background-color: $backgroundColor;
  height: 100%;
  width: 100%;
}
</style>
