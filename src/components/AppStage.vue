<template>
  <main>
    <h2>Make a Color in HSL</h2>

    <div class="color-display" :style="colorDisplay"><p><strong>HSL:</strong> {{ hue }}° {{ saturation }}% {{ lightness }}%</p></div>

    <div class="color-variants">
      <span><strong>HEX:</strong> {{ hex }}</span>
      <span><strong>RGB:</strong> {{ rgb }}</span>
    </div>

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
        value="90"
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

    <PaletteDisplay/>
  </main>
</template>

<script>
import chroma from "chroma-js";
import PaletteDisplay from './PaletteDisplay.vue';

export default {
  name: 'AppStage',
  components: {
    PaletteDisplay
  },
  data() {
    return {
      hue: 0,
      saturation: 90,
      lightness: 50,
      scale: [],
      steps: 5,
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
    hslToHex(h, s, l) {
      l /= 100;
      const a = s * Math.min(l, 1 - l) / 100;
      const f = n => {
        const k = (n + h / 30) % 12;
        const color = l - a * Math.max(Math.min(k - 3, 9 - k, 1), -1);
        return Math.round(255 * color).toString(16).padStart(2, '0');   // convert to Hex and prefix "0" if needed
      };
      return `#${f(0)}${f(8)}${f(4)}`;
    },
  },
  computed: {
    colorDisplay() {
      return `background-color: ${this.hslDecl}`
    },
    hslDecl() {
      return `hsl(${this.hue} ${this.saturation}% ${this.lightness}%)`
    },
    hslVals() {
      return `${this.hue} ${this.saturation} ${this.lightness}`
    },
    hex() {
      return this.hslToHex(this.hue, this.saturation, this.lightness)
    },
    rgb() {
      return chroma(this.hex).rgb()
    },
  },
  mounted() {
    this.hue = this.getRandInt(0, 359);
    document.getElementById('hue-input').value = this.hue;
  },
  watch: {
    hslVals() {

    }
  },
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
