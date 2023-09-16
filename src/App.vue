<template>
  <AppStage/>
</template>

<script>
import AppStage from './components/AppStage.vue'

export default {
  name: 'App',
  components: {
    AppStage
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Exo:wght@300;700&display=swap');

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background-color:#ccc;
}

p, span, a, div, h1, h2, h3, h4, h5 {
  font-family: 'Exo', sans-serif;
}

h1, h2, h3, h4, h5 {
  font-weight: 700;
}
</style>
