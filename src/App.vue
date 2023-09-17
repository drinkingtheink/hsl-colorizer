<template>
  <AppStage :lightOrDark="lightOrDark"/>
  <footer>
    <a href="https://en.wikipedia.org/wiki/HSL_and_HSV">About the HSL Color Space</a>
    <a href="https://github.com/drinkingtheink/hsl-colorizer">About this App</a>
    <a href="https://www.drinkingtheink.com/?topic=about">About the Author</a>
  </footer>
</template>

<script>
import AppStage from './components/AppStage.vue'

export default {
  name: 'App',
  components: {
    AppStage
  },
  methods: {
    lightOrDark(color) {
      // Variables for red, green, blue values
      var r, g, b, hsp;
      
      // Check the format of the color, HEX or RGB?
      if (color.match(/^rgb/)) {

          // If RGB --> store the red, green, blue values in separate variables
          color = color.match(/^rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*(\d+(?:\.\d+)?))?\)$/);
          
          r = color[1];
          g = color[2];
          b = color[3];
      } 
      else {
          // If hex --> Convert it to RGB: http://gist.github.com/983661
          color = +("0x" + color.slice(1).replace( 
          color.length < 5 && /./g, '$&$&'));

          r = color >> 16;
          g = color >> 8 & 255;
          b = color & 255;
      }
      
      // HSP (Highly Sensitive Poo) equation from http://alienryderflex.com/hsp.html
      hsp = Math.sqrt(
      0.299 * (r * r) +
      0.587 * (g * g) +
      0.114 * (b * b)
      );

      // Using the HSP value, determine whether the color is light or dark
      if (hsp>127.5) {
          return 'light';
      } 
      else {
          return 'dark';
      }
  },
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Exo:wght@300;700&display=swap');

html, body {
  width: 100vw;
}

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

footer {
  padding-top: 4rem;
}

footer a {
  display: inline-block;
  color: var(--primary);
  padding: 1rem 2rem;
  background-color: var(--transBg);
  margin: 0 5px 5px 0;
  color: white;
  text-decoration: none;
  transition: all 0.5s;
  border-left: 5px solid var(--primary);
  transition: all 0.5s;
  width: 200px;
}

footer a:hover {
  background-color: var(--primary);
  border-color: white;
  text-shadow: 1px 1px 2px black;
}
</style>
