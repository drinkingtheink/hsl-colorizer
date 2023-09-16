<template>
  <main>
    <div class="arrow-down" />

    <h2 class="app-title" :class="lightOrDark(hex)">HSL Color Explorer</h2>

    <section class="config">
      <div class="small-color-display" />
      <div class="wrapper hue">
        <input 
          type="range" 
          id="hue-input" 
          name="hue-input" 
          min="0" 
          max="359" 
          @input="updateHue"
        />
        <label for="hue-input"><strong>Hue</strong> {{ hue }}°</label>
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
        <label for="sat-input"><strong>Saturation</strong> {{ saturation }}%</label>
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
        <label for="light-input"><strong>Lightness</strong> {{ lightness }}%</label>
      </div>
    </section>


    <div 
      class="color-display" 
      :style="colorDisplay"
      @click="focusHueInput"
    >
      <p>
        <strong>HSL:</strong> 
        <span class="hue-display">{{ hue }}°</span> 
        <span class="sat-display">{{ saturation }}%</span> 
        <span class="light-display">{{ lightness }}%</span>
      </p>
    </div>

    <div class="color-variants">
      <span class="hex"><strong>HEX:</strong> <span class="hex-display">{{ hex }}</span></span>
      <span class="rgb"><strong>RGB:</strong> <span class="rgb-display">{{ rgb }}</span></span>
    </div>

    <PaletteDisplay :hex="hex" :focusHueInput="focusHueInput" />
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
    focusHueInput() {
      const hueInput = document.getElementById('hue-input')

      if (hueInput) hueInput.focus()
    }
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
    hex() {
      document.documentElement.style.setProperty('--primary', this.hex);
    }
  }
}
</script>

<style>
:root {
  --flatBg: #e1e1e1;
  --transBg: rgba(0,0,0,0.7);
  --transBgW: rgba(255,255,255,0.7);
  --customMixInputWidth: 300px;
  --borRad: 10px;
  --pionterDim: 200px;
  --customMixCircDim: 50px;
}

html, body {
  padding: 0;
  margin: 0;
}

main {
  border-top: 10px solid var(--primary);
}

.app-title {
  max-width: 300px;
  margin: 1rem auto;
  padding: 10px;
  position:relative;
  z-index: 2;
  transition: all 1s;
}

.arrow-down {
  margin: 0 auto;
  width: 0; 
  height: 0; 
  border-left: var(--pionterDim) solid transparent;
  border-right: var(--pionterDim) solid transparent;
  border-top: var(--pionterDim) solid var(--primary);
  position: absolute;
  top: 0;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
}

.app-title {
  background-color: var(--transBgW);
}

.app-title.dark {
  color: white;
  text-shadow: 0 0 10px var(--transBg);
  background-color: var(--transBg);
}

.color-display {
  padding: 4rem;
  margin: 0.5rem auto 0 auto;
  width: 20rem;
  border-radius: 10px;
  border: 10px solid rgba(0,0,0,0.3);
}

.color-display p {
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 5px 10px;
  border-radius: 10px;
  font-size: 120%;
}

.color-display span {
  width:40px;
  display: inline-block;
  margin-right: 10px;
}

.color-variants {
  display: flex;
  justify-content: center;
  margin: 0 auto;
  padding: 0.5rem 0;
}

.color-variants span {
  margin: 0 10px;
  padding: 5px 10px;
  border-radius: 10px;
  color: white;
  background-color: rgba(0, 0, 0, 0.7);
}

.color-variants span span {
  background-color: transparent;
  display: inline-block;
}

.color-variants span span.hex-display {
  width: 40px;
}

.color-variants span span.rgb-display {
  width: 130px;
}

.color-variants .hex {
  width: 150px;
}

.color-variants .rgb {
  width: 210px;
}

.config {
  display: flex;
  justify-content: center;
  position: sticky;
  top: 0;
  background-color: rgba(0,0,0,0.5);
  padding: 10px 0;
  z-index: 100000000;
}

.config div {
  margin-right: 10px;
  width: 140px;
  padding: 10px;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  border-radius: 10px;
}

.config .small-color-display {
  width: 3px;
  height: 3px;
  border-radius: 50%;
  background-color: var(--primary);
  border: 4px solid rgba(0, 0, 0, 0.4);
  margin-top: 18px;
}
</style>
