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
          :value="hue"
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
          :value="saturation"
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
          :value="lightness" 
          @input="updateLight"
        />
        <label for="light-input"><strong>Lightness</strong> {{ lightness }}%</label>
      </div>
    </section>

    <section class="color-display-stage">
      <div class="color-variants">
        <span class="hex"><strong>HEX:</strong> <span class="hex-display">{{ hex }}</span></span>
        <span class="rgb"><strong>RGB:</strong> <span class="rgb-display">{{ rgb }}</span></span>
      </div>
      <div 
        class="color-display" 
        :style="colorDisplay"
        @click="focusHueInput"
      >
        <p> 
          <span class="hue-display"><strong>H:</strong> {{ hue }}°</span> 
          <span class="sat-display"><strong>S:</strong> {{ saturation }}%</span> 
          <span class="light-display"><strong>L:</strong> {{ lightness }}%</span>
        </p>
      </div>

      <div class="suggestions">
        <h3>Or Try One of These:</h3>
          <div>
              <span
                  class="sugg"
                  v-for="sugg, index in suggestions"
                  :key="`${sugg}-${index}`"
                  :style="`background-color: ${sugg}`"
                  @click="handleSuggClick(sugg)"
              />
          </div>
      </div>

    </section>

    <div class="color-actions">
      <button 
        id="get-random-color" 
        @click="randomizeHue"
        :class="lightOrDark(hex)"
      >Use Random Color</button>
    </div>

    <PaletteDisplay 
      :hex="hex" 
      :focusHueInput="focusHueInput" 
      :lightOrDark="lightOrDark"
      :mixColorFromURL="mixQuery"
      @mixUpdate="updateMixQueryString"
      @hslUpdate="handleHSLUpdateFromPalettes"
    />

    <ColorDisplay 
      :hex="hex" 
      :hue="hue" 
      :sat="saturation" 
      :light="lightness" 
    />
  </main>
</template>

<script>
import chroma from "chroma-js";
import PaletteDisplay from './PaletteDisplay.vue';
import ColorDisplay from './ColorDisplay.vue';

const maxHue = 359
const maxSat = 100
const maxLit = 100

export default {
  name: 'AppStage',
  components: {
    PaletteDisplay,
    ColorDisplay
  },
  props: {
    lightOrDark: Function,
  },
  data() {
    return {
      hue: 0,
      saturation: 90,
      lightness: 50,
      suggestions: [],
      suggestionCount: 12,
      mixQuery: null,
    }
  },
  methods: {
    getValsFromHSL(hsl) {
      let h = hsl[0].toFixed()
      const s = (hsl[1] * 100).toFixed()
      const l = (hsl[2] * 100).toFixed()

      if (isNaN(h)) {
        h = 1
      }

      return { h, s, l }
    },
    handleHSLUpdateFromPalettes(hsl) {
      const { h, s, l } = this.getValsFromHSL(hsl)

      const hueEv = {
        target: {
          value : h
        }
      }

      this.updateHue(hueEv)

      const satEv = {
        target: {
          value : s
        }
      }

      this.updateSat(satEv)

      const litEv = {
        target: {
          value : l
        }
      }

      this.updateLight(litEv)
    },
    handleSuggClick(color) {
      const theColor = chroma(color)
      const hslVal = theColor.hsl()

      const { h, s, l } = this.getValsFromHSL(hslVal)

      const hEvent = {
        target: {
          value: h
        }
      }

      this.updateHue(hEvent)

      const sEvent = {
        target: {
          value: s
        }
      }

      this.updateSat(sEvent)

      const lEvent = {
        target: {
          value: l
        }
      }

      this.updateLight(lEvent)
      
      this.generateColorSuggestions()
    },
    generateColorSuggestions() {
        for (var i = 0; i < this.suggestionCount; i++) {
            this.suggestions[i] = chroma.random()
        }
    },
    checkForQueryStrings() {
      let queryParams = new URLSearchParams(window.location.search)
      let hQuery = queryParams.get('hue')
      let sQuery = queryParams.get('sat')
      let lQuery = queryParams.get('lit')
      let mixQuery = queryParams.get('mix')

      if (hQuery) {
        this.hue = Number(hQuery)
      } else {
        console.log(`No hue query found so setting randomly...`)
        this.hue = this.getRandInt(0, 359)
      }

      if (sQuery) {
        this.saturation = Number(sQuery)
      } else {
        this.updateSatQueryString(90)
      }

      if (lQuery) {
        this.lightness = Number(lQuery)
      } else {
        this.updateLitQueryString(this.lightness)
      }

      if (mixQuery === "null") {
        this.mixQuery = null
        this.deleteMixQuery()
      } else if (mixQuery) {
        this.mixQuery = mixQuery
      } 
    },
    updateHue(e) {
      this.hue = Number(e.target.value)

      this.updateHueQueryString(this.hue)
    },
    updateSat(e) {
      this.saturation = Number(e.target.value)

      this.updateSatQueryString(this.saturation)
    },
    updateLight(e) {
      this.lightness = Number(e.target.value)

      this.updateLitQueryString(this.lightness)
    },
    deleteMixQuery() {
      const urlObj = new URL(window.location);
      urlObj.searchParams.delete('mix');
      
      return urlObj.toString();
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
    focusHueInput() {
      const hueInput = document.getElementById('hue-input')

      if (hueInput) hueInput.focus()
    },
    updateHueQueryString(val) {
      let queryParams = new URLSearchParams(window.location.search);
      queryParams.set(`hue`, val);
      history.replaceState(null, null, `?${queryParams.toString()}`);
    },
    updateSatQueryString(val) {
      let queryParams = new URLSearchParams(window.location.search);
      queryParams.set(`sat`, val);
      history.replaceState(null, null, `?${queryParams.toString()}`);
    },
    updateLitQueryString(val) {
      let queryParams = new URLSearchParams(window.location.search);
      queryParams.set(`lit`, val);
      history.replaceState(null, null, `?${queryParams.toString()}`);
    },
    updateMixQueryString(val) {
      let queryParams = new URLSearchParams(window.location.search);
      queryParams.set(`mix`, val);
      history.replaceState(null, null, `?${queryParams.toString()}`);
    },
    randomizeHue() {
      this.hue = this.getRandInt(0, maxHue)
      this.saturation = 90
      this.lightness = 50
      this.generateColorSuggestions()
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
    this.checkForQueryStrings()
    this.generateColorSuggestions()

    const hueInput = document.getElementById('hue-input')
    if (hueInput) document.getElementById('hue-input').value = this.hue
  },
  watch: {
    hex() {
      document.documentElement.style.setProperty('--primary', this.hex)
    },
    hue() {
      if (this.hue > maxHue) this.hue = maxHue

      this.updateHueQueryString(this.hue)
    },
    saturation() {
      if (this.saturation > maxSat) this.saturation = maxSat

      this.updateSatQueryString(this.saturation)
    },
    lightness() {
      if (this.lightness > maxLit) this.lightness = maxLit
5
      this.updateLitQueryString(this.lightness)
    },
  }
}
</script>

<style>
:root {
  --flatBg: #e1e1e1;
  --transBg: rgba(0,0,0,0.7);
  --boxShad: 0 0 10px var(--transBg);
  --transBgW: rgba(255,255,255,0.7);
  --customMixInputWidth: 300px;
  --borRad: 10px;
  --pointerDim: 200px;
  --customMixCircDim: 70px;
  --darkGrey: #222;
  --appWidth: 1200px;
  --swatchBord: 1px solid rgba(255,255,255,0.3);
  --swatchDim: 100px;
  --lrdMobSwatchDim: 50px;
  --suggDim: 25px;
  --mobMinWidth: 500px;
  --smallColorDim: 30px;
  --qmarkColor1: #abfcc7;
  --qmarkColor2: #FAE282;
  --qmarkColor3: #BCC7FF;
}

html, body {
  padding: 0 0 10rem 0;
  margin: 0;
}

main {
  border-top: 10px solid var(--primary);
  position: relative;
}

.suggestions {
  width: 250px;
  padding-left: 20px;
  margin-top: 2rem;
}

.sugg {
  display: inline-block;
  height: var(--suggDim);
  width: var(--suggDim);
  margin-right: 4px;
  border-radius: 50%;
  border: 4px solid rgba(0,0,0,0.3);
  transition: all 0.2s;
}

.suggestions h3 {
  font-size: 90%;
}

.sugg:hover {
  transform: scale(1.1);
  cursor: pointer;
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
  border-left: var(--pointerDim) solid transparent;
  border-right: var(--pointerDim) solid transparent;
  border-top: var(--pointerDim) solid var(--primary);
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

.color-display-stage {
  position: relative;
  display: flex;
  justify-content: center;
  width: 100vw;
}

.color-display {
  padding: 4rem;
  margin: 0.5rem 0 1rem 0;
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
  margin-right: 30px;
}

.color-variants {
  width: 250px;
  padding-top: 4.5rem;
  padding-right: 20px;
}

.color-variants span {
  margin: 0 0 10px 0;
  padding: 2px;
  border-radius: 10px;
  color: white;
  background-color: rgba(0, 0, 0, 0.7);
  display: block;
}

.color-variants span span {
  background-color: transparent;
  display: inline-block;
}

.color-variants span span.hex-display {
  width: 90px;
}

.color-variants span span.rgb-display {
  width: 130px;
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
  width: var(--smallColorDim);
  height: var(--smallColorDim);
  border-radius: 50%;
  background-color: var(--primary);
  border: 4px solid rgba(0, 0, 0, 0.4);
}

.section-banner {
  width: 100vw;
  background-color: var(--transBg);
  color: white;
  padding: 0.5rem 0;
  border-top: 4px solid var(--primary);
}

#get-random-color {
  color: white;
  margin-left: 10px;
}

#get-random-color.light:hover {
  color: var(--darkGrey);
}

#get-random-color:hover {
  background-color: var(--primary)
}

@media (width <= 800px) {
  .suggestions {
    width: 100%;
    margin: 1rem 0;
    width: 90%;
  }

  .color-display-stage {
    display: block;
  }

  .color-display {
    margin: 0 auto 1rem auto;
  }

  .color-variants {
    width: 100%;
    padding-top: 1rem;
  }

  .color-variants span {
    display: inline-block;
    padding-right: 10px;
    padding-left: 10px;
    margin-right: 10px;
  }
}

@media (width <= 740px) {
    #clear-custom-color {
      display: block;
      margin: 0 auto;
    }
}

@media (width <= 550px) {
  :root {
    --swatchDim: 80px;
  }

  html, body {
    width: 100vw;
  }

  .config {
    flex-wrap: wrap;
  }

  .config div {
    width: 100px;
    font-size: 80%;
    margin-bottom: 5px;
  }

  .config input {
    width: 90px;
  }

  .color-display {
    margin-top: 1rem;
    width: 60%;
  }

  .color-display span {
    margin-right: 20px;
  }

  .color-variants {
    display: block;
  }

  .color-variants span {
    margin: 0 5px 5px 0;
  }

  #get-random-color {
    display: block;
    width: 90%;
    margin: 1rem auto 0 auto;
  }

  .gallery-wrapper {
    width: 100%;
  }

  .gallery-wrapper.custom {
    width: 100%;
  }

  button.mix-random.btn2 {
    display: block;
    width: 80%;
    margin: 1rem auto 0 auto;
  }

  .color-captured {
    width: 85%;
    margin: 0 auto;
  }

  .change-color .custom-actions {
    position: static;
    padding-top: 1rem;
  }

  .light-to-dark .swatch {
    width: var(--lrdMobSwatchDim);
    height: var(--lrdMobSwatchDim);
  }

  .small-color-display {
    display: block;
    width: 90% !important;
    border-radius: 3px !important;
    height: 6px !important;
    margin: 0 auto 0.5rem auto !important;
    padding: 2px !important;
  }

  .palette-stage {
    overflow: hidden;
  }

  .sugg:nth-child(n+8) {
    display: none;
  }
}

@media (width <= 390px) {
  .config div {
    width: 90px;
    font-size: 75%;
  }

  .config .wrapper label strong {
    display: block;
  }
}
</style>
