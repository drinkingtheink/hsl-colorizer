<template>
  <section class="colors-stage">
    <h2>Colors Time!</h2>
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
          complimentary: null,
          split: null,
          triad: null,
          tetrad: null,
          analogous: null,
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
        this.harmonizeColor(this.hue, this.sat, this.light)
    },
    light() {
        this.harmonizeColor(this.hue, this.sat, this.light)
    },
  },
}
</script>

<style>

</style>
