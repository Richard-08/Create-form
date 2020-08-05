<template>
  <div id="app">
    <div class="container">
      <Form />
    </div>
  </div>
</template>

<script>
import Form from './components/Form';

export default {
  name: 'App',
  components: {
    Form,
  },
};
</script>

<style lang="scss">
@import './assets/scss/_stylebase.scss';

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: $mainFontColor;
  font-family: $mainFont;
  font-size: $mainFontSize;
}

.container {
  max-width: 100%;
  width: 1280px;
  margin: 0 auto;
  padding: 20px;
}
</style>
