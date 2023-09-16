<template>
  <section class="colors-stage">
    <h3 class="section-banner">Color Collections</h3>

    <div class="flex-wrapper">
        <div class="complementary">
            <div v-if="complementary.length > 0" class="gallery-wrapper complementary">
                <h3>complementary</h3>
                <div class="gallery">
                    <span
                        class="swatch"
                        v-for="swatch in complementary"
                        :key="swatch"
                        :style="`background-color: ${swatch}`"
                    ><span>{{ swatch }}</span></span>
                </div>
            </div>
        </div>

        <div class="split">
            <div v-if="split.length > 0" class="gallery-wrapper split">
                <h3>Split</h3>
                <div class="gallery">
                    <span
                        class="swatch"
                        v-for="swatch in split"
                        :key="swatch"
                        :style="`background-color: ${swatch}`"
                    ><span>{{ swatch }}</span></span>
                </div>
            </div>
        </div>

        <div class="triad">
            <div v-if="triad.length > 0" class="gallery-wrapper triad">
                <h3>Triad</h3>
                <div class="gallery">
                    <span
                        class="swatch"
                        v-for="swatch in triad"
                        :key="swatch"
                        :style="`background-color: ${swatch}`"
                    ><span>{{ swatch }}</span></span>
                </div>
            </div>
        </div>

        <div class="tetrad">
            <div v-if="tetrad.length > 0" class="gallery-wrapper tetrad">
                <h3>Tetrad</h3>
                <div class="gallery">
                    <span
                        class="swatch"
                        v-for="swatch in tetrad"
                        :key="swatch"
                        :style="`background-color: ${swatch}`"
                    ><span>{{ swatch }}</span></span>
                </div>
            </div>
        </div>

        <div class="analogous">
            <div v-if="analogous.length > 0" class="gallery-wrapper analogous">
                <h3>Analogous</h3>
                <div class="gallery">
                    <span
                        class="swatch"
                        v-for="swatch in analogous"
                        :key="swatch"
                        :style="`background-color: ${swatch}`"
                    ><span>{{ swatch }}</span></span>
                </div>
            </div>
        </div>
    </div>
  </section>
</template>

<script>
// import chroma from "chroma-js";

export default {
  name: 'ColorDisplay',
  props: {
      hex: String,
      hue: Number,
      sat: Number,
      light: Number,
  },
  data() {
      return {
          complimentary: [],
          split: [],
          triad: [],
          tetrad: [],
          analogous: [],
      }
  },
  methods: {
    harmonize(color, h, s, l, start, end, interval) {
        const colors = [color]

        for(let i = start; i <= end; i += interval) {
            const h1 = (h + i) % 360
            const c1 = `hsl(${h1}, ${s}%, ${l}%)`
            colors.push(c1)
        }

        return colors
    },
    harmonizeColor(hex, h, s, l) {
        this.complimentary = this.harmonize(hex, h,s,l, 180, 180, 1)
        this.split = this.harmonize(hex, h,s,l, 150, 210, 60)
        this.triad = this.harmonize(hex, h,s,l, 120, 240, 120)
        this.tetrad = this.harmonize(hex, h,s,l, 90, 270, 90)
        this.analogous = this.harmonize(hex, h,s,l, 30, 90, 30)
    },
  },
  mounted() {
    this.harmonizeColor(this.hex, this.hue, this.sat, this.light)
  },
  watch: {
    hue() {
        this.harmonizeColor(this.hex, this.hue, this.sat, this.light)
    },
    sat() {
        this.harmonizeColor(this.hex, this.hue, this.sat, this.light)
    },
    light() {
        this.harmonizeColor(this.hex, this.hue, this.sat, this.light)
    },
  },
}
</script>

<style>
.flex-wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    max-width: var(--appWidth);
    margin: 0 auto;
    padding: 0 0 1rem 0;
}

.colors-stage .gallery-wrapper {
    margin: 10px 20px 50px 20px;
}
</style>
