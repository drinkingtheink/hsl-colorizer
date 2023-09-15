<template>
  <main>
    <h2>Make a Color in HSL</h2>

    <div class="color-display" :style="colorDisplay"><p>{{ hslDecl }}</p></div>

    <div class="wrapper hue">
      <input 
        type="range" 
        id="hue-input" 
        name="hue-input" 
        min="0" 
        max="359" 
        @input="updateHue"
      />
      <label for="hue-input">Hue ({{ hue }})</label>
    </div>

    <div class="wrapper sat">
      <input 
        type="range" 
        id="sat-input" 
        name="sat-input" 
        min="0" 
        max="100" 
        @input="updateSat"
      />
      <label for="sat-input">Saturation ({{ saturation }})</label>
    </div>

    <div class="wrapper light">
      <input 
        type="range" 
        id="light-input" 
        name="light-input" 
        min="0" 
        max="100" 
        value="50" 
        @input="updateLight"
      />
      <label for="light-input">Lightness ({{ lightness }})</label>
    </div>
  </main>
</template>

<script>
// import chroma from "chroma-js";

export default {
  name: 'AppStage',
  data() {
    return {
      hue: 0,
      saturation: 100,
      lightness: 50,
      scale: [],
    }
  },
  methods: {
    updateHue(e) {
      this.hue = e.target.value;
    },
    updateSat(e) {
      this.saturation = e.target.value;
    },
    updateLight(e) {
      this.lightness = e.target.value;
    },
    getRandInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
  },
  computed: {
    colorDisplay() {
      return `background-color: ${this.hslDecl}`
    },
    hslDecl() {
      return `hsl(${this.hue} ${this.saturation}% ${this.lightness}%)`
    },
  },
  mounted() {
    this.hue = this.getRandInt(0, 359);
    document.getElementById('hue-input').value = this.hue;
  }
}
</script>

<style>
.color-display {
  padding: 4rem;
  margin: 2rem auto 0 auto;
  width: 20rem;
  border-radius: 10px;
}

.color-display p {
  background-color: rgba(0, 0, 0, 0.6);
  color: white;
  padding: 5px 10px;
  border-radius: 10px;
}
</style>
