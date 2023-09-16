<template>
  <section class="palette-stage">
    <h2>Palettes:</h2>
  </section>
</template>

<script>
// import chroma from "chroma-js";

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
  }
}
</script>

<style>

</style>
