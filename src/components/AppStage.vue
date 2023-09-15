<template>
  <main>
    <section>
    <input type="color" @input="updateactiveHex" />
    </section>
    <section>
      <div v-show="activeHex" class="active-color-display hex" :style="activeHexDisplay" />

      <div v-show="activeRGB" class="rgb">
        <p>{{ activeRGB }}</p>
      </div>

      <div v-show="activeHSL" class="hsl">
        <p>{{ activeHSL }}</p>

        <div v-show="activeHSL" class="active-color-display hsl" :style="{ 'background-color': 'hsl(' + activeHSL + ')' }" />
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
        60 * h < 0 ? 60 * h + 360 : 60 * h,
        100 * (s ? (l <= 0.5 ? s / (2 * l - s) : s / (2 - (2 * l - s))) : 0),
        (100 * (2 * l - s)) / 2,
      ];
    },
  },
  computed: {
    activeHexDisplay() {
      return `background: ${this.activeHex}`;
    },
    activeRGB() {
      return this.hexToRgb(this.activeHex);
    }
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
