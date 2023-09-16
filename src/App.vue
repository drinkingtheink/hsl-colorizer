<template>
  <AppStage/>
</template>

<script>
import AppStage from './components/AppStage.vue'

export default {
  name: 'App',
  components: {
    AppStage
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Exo:wght@300;700&display=swap');

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

body {
background-color: #e1e1e1;
background-image: url("data:image/svg+xml,%3Csvg width='84' height='48' viewBox='0 0 84 48' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0 0h12v6H0V0zm28 8h12v6H28V8zm14-8h12v6H42V0zm14 0h12v6H56V0zm0 8h12v6H56V8zM42 8h12v6H42V8zm0 16h12v6H42v-6zm14-8h12v6H56v-6zm14 0h12v6H70v-6zm0-16h12v6H70V0zM28 32h12v6H28v-6zM14 16h12v6H14v-6zM0 24h12v6H0v-6zm0 8h12v6H0v-6zm14 0h12v6H14v-6zm14 8h12v6H28v-6zm-14 0h12v6H14v-6zm28 0h12v6H42v-6zm14-8h12v6H56v-6zm0-8h12v6H56v-6zm14 8h12v6H70v-6zm0 8h12v6H70v-6zM14 24h12v6H14v-6zm14-8h12v6H28v-6zM14 8h12v6H14V8zM0 8h12v6H0V8z' fill='%23b8b4bf' fill-opacity='0.4' fill-rule='evenodd'/%3E%3C/svg%3E");
}
p, span, a, div, h1, h2, h3, h4, h5 {
  font-family: 'Exo', sans-serif;
}

h1, h2, h3, h4, h5 {
  font-weight: 700;
}

label {
  display: block;
}

button {
  background-color: var(--transBg);
  color: var(--primary);
  border: none;
  padding: 10px 20px;
  text-transform: uppercase;
  border-radius: var(--borRad);
  transition: all 1s;
  box-shadow: 0 0 10px var(--primary);
}

button:hover {
  color: white;
  box-shadow: 0 0 20px var(--primary);
}
</style>
