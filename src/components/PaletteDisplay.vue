<template>
  <section class="palette-stage">
    <h2>Palettes:</h2>
    <div v-if="lightArray.length > 0" class="gallery light">
        <h3>Trending Lighter</h3>
        <span
            class="swatch"
            v-for="swatch in lightArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
    </div>

    <div v-if="darkArray.length > 0" class="gallery dark">
        <h3>Trending Darker</h3>
        <span
            class="swatch"
            v-for="swatch in darkArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
    </div>

    <div v-if="toBlueArray.length > 0" class="gallery to-blue">
        <h3>Mix to Blue</h3>
        <span
            class="swatch"
            v-for="swatch in toBlueArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
    </div>

    <div v-if="toRedArray.length > 0" class="gallery to-red">
        <h3>Mix to Red</h3>
        <span
            class="swatch"
            v-for="swatch in toRedArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
    </div>

    <div v-if="toYellowArray.length > 0" class="gallery to-yellow">
        <h3>Mix to Yellow</h3>
        <span
            class="swatch"
            v-for="swatch in toYellowArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
    </div>

    <div v-if="randomArray.length > 0" class="gallery random">
        <h3>Random Selection</h3>
        <span
            class="swatch"
            v-for="swatch in randomArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
    </div>
  </section>
</template>

<script>
import chroma from "chroma-js";

export default {
  name: 'PaletteDisplay',
  props: {
      hex: String
  },
  data() {
      return {
          steps: 5,
          darkArray: [],
          lightArray: [],
          randomArray: [],
          lumArray: [],
          toBlueArray: [],
          toRedArray: [],
          toYellowArray: [],
      }
  },
  methods: {
    makePalettes() {
        this.makeDarkArray()
        this.makeLightArray()
        this.makeRandomArray()
        this.makeToBlueArray()
        this.makeToRedArray()
        this.makeToYellowArray()
    },
    makeDarkArray() {
        const chromaColor = chroma(this.hex)

        for (var i = 0; i < this.steps; i++) {
            this.darkArray[i] = chromaColor.darken(i)
        }
    },
    makeLightArray() {
        const chromaColor = chroma(this.hex)

        for (var i = 0; i < this.steps; i++) {
            this.lightArray[i] = chromaColor.brighten(i)
        }
    },
    makeRandomArray() {
        for (var i = 0; i < this.steps; i++) {
            this.randomArray[i] = chroma.random()
        }

        this.randomArray[0] = this.hex
    },
    makeToBlueArray() {
        const chromaColor = chroma(this.hex)

        for (var i = 0; i < this.steps; i++) {
            this.toBlueArray[i] = chroma.mix(chromaColor, 'blue', i * 0.35)
        }
    },
    makeToRedArray() {
        const chromaColor = chroma(this.hex)

        for (var i = 0; i < this.steps; i++) {
            this.toRedArray[i] = chroma.mix(chromaColor, 'red', i * 0.35)
        }
    },
    makeToYellowArray() {
        const chromaColor = chroma(this.hex)

        for (var i = 0; i < this.steps; i++) {
            this.toYellowArray[i] = chroma.mix(chromaColor, 'yellow', i * 0.35)
        }
    },
  },
  mounted() {
      if (this.hex) this.makePalettes()
  },
  watch: {
    hex() {
        this.makePalettes()
    }
  },
}
</script>

<style>
.gallery {
    display: flex;
    margin: 0 auto;
    max-width: 800px;
    justify-content: center;
}

.swatch {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    margin-right: 5px;
}
</style>
