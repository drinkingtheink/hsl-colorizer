<template>
  <main>
    <section>
      <h2>Color Selection</h2>
      <input type="color" @input="updateactiveHex" />
    </section>
    <section>
      <div v-show="activeHex" class="active-color-display hex" :style="activeHexDisplay" />

      <div class="rgb">
        <p>{{ activeRGB }}</p>
      </div>

      <div class="hsl">
        <p>{{ activeHSL }}</p>

        <div class="active-color-display hsl" :style="activeHSLDisplay" />
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: 'AppStage',
  data() {
    return {
      activeHex: null,
      activeHSL: null,
    }
  },
  methods: {
    updateactiveHex(e) {
      this.activeHex = e.target.value;
      this.deriveHSL();
    },
    hexToRgb(hex) {
      return hex ? hex.replace(/^#?([a-f\d])([a-f\d])([a-f\d])$/i
                ,(m, r, g, b) => '#' + r + r + g + g + b + b)
        .substring(1).match(/.{2}/g)
        .map(x => parseInt(x, 16)) : null
    },
    deriveHSL() {
      this.activeRGB ? this.activeHSL = this.rgbToHSL(this.activeRGB) : null;
    },
    rgbToHSL ([r, g, b]) {
      r /= 255;
      g /= 255;
      b /= 255;
      const l = Math.max(r, g, b);
      const s = l - Math.min(r, g, b);
      const h = s
        ? l === r
          ? (g - b) / s
          : l === g
          ? 2 + (b - r) / s
          : 4 + (r - g) / s
        : 0;
      return [
        Math.round(60 * h < 0 ? 60 * h + 360 : 60 * h),
        Math.round(100 * (s ? (l <= 0.5 ? s / (2 * l - s) : s / (2 - (2 * l - s))) : 0)) + '%',
        Math.round((100 * (2 * l - s)) / 2) + '%',
      ];
    },
  },
  computed: {
    activeHexDisplay() {
      return `background: ${this.activeHex}`;
    },
    activeRGB() {
      return this.hexToRgb(this.activeHex);
    },
    activeHSLDisplay() {
      return `background-color: ${this.activeHSL}`;
    },
  },
}
</script>

<style>
.active-color-display {
  padding: 4rem;
  margin: 2rem auto 0 auto;
  width: 20rem;
  border-radius: 10px;
}
</style>
