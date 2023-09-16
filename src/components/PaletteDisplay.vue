<template>
  <section class="palette-stage">
    <h2>Palettes:</h2>

    <div v-if="darkArray.length > 0" class="gallery dark">
        <span
            class="swatch"
            v-for="swatch in lightArray"
            :key="swatch"
            :style="`background-color: ${swatch}`"
        />
        <span
            class="swatch"
            v-for="swatch in darkArray"
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
      }
  },
  methods: {
    makePalettes() {
        this.makeDarkArray();
        this.makeLightArray();
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
  },
  mounted() {
      if (this.hex) this.makePalettes()
  }
}
</script>

<style>
.gallery {
    display: flex;
}

.swatch {
    width: 40px;
    height: 40px;
    border-radius: 50%;
}
</style>
