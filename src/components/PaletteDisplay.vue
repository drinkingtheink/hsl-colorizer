<template>
  <section class="palette-stage">
    <div v-if="lightArray.length > 0 && darkArray.length > 0" class="gallery-wrapper light-to-dark">
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in lightArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
            <span
                class="swatch"
                v-for="swatch in darkArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div class="gallery-wrapper custom">
        <h3>Make Your Own Mix</h3>
        <div v-if="!customMixColor" class="capture-color">
            <p class="romance"><span class="selected" @click="focusHueInput" /><span class="and">&</span><span class="q-mark">?</span><span class="q-mark">?</span><span class="q-mark">?</span></p>
            <label for="custom-color-input">Pick a color below to mix with your selection</label>
            <input type="color" id="custom-color-input" name="custom-color-input" @input="handleCustomColorInput" />
        </div>
        <div v-if="customMixColor" class="color-captured">
            <div class="gallery">
                <span
                    class="swatch"
                    v-for="swatch in customArray"
                    :key="swatch"
                    :style="`background-color: ${swatch}`"
                ><span>{{ swatch }}</span></span>
            </div>
            <div class="change-color">
                <label for="custom-color-input">Choose Another Color to Change Your Mix</label>
                <input type="color" id="custom-color-input" name="custom-color-input" @input="handleCustomColorInput" :value="customMixColor" />
                <button @click="clearCustomColor">Clear Selection</button>
            </div>
        </div>
    </div>

    <div v-if="toBlueArray.length > 0" class="gallery-wrapper to-blue">
        <h3>Mix to Blue</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toBlueArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toRedArray.length > 0" class="gallery-wrapper to-red">
        <h3>Mix to Red</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toRedArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toYellowArray.length > 0" class="gallery-wrapper to-yellow">
        <h3>Mix to Yellow</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toYellowArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toGreenArray.length > 0" class="gallery-wrapper to-green">
        <h3>Mix to Green</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toGreenArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toOrangeArray.length > 0" class="gallery-wrapper to-orange">
        <h3>Mix to Orange</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toOrangeArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toPurpleArray.length > 0" class="gallery-wrapper to-purple">
        <h3>Mix to Purple</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toPurpleArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toBlueGreenArray.length > 0" class="gallery-wrapper to-blue-green">
        <h3>Mix to Blue-Green</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toBlueGreenArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toBlueVioletArray.length > 0" class="gallery-wrapper to-blue-violet">
        <h3>Mix to Blue-Violet</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toBlueVioletArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toRedOrangeArray.length > 0" class="gallery-wrapper to-red-orange">
        <h3>Mix to Red-Orange</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toRedOrangeArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toRedVioletArray.length > 0" class="gallery-wrapper to-red-violet">
        <h3>Mix to Red-Violet</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toRedVioletArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toYellowOrangeArray.length > 0" class="gallery-wrapper to-yellow-orange">
        <h3>Mix to Yellow-Orange</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toYellowOrangeArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>

    <div v-if="toYellowGreenArray.length > 0" class="gallery-wrapper to-yellow-green">
        <h3>Mix to Yellow-Green</h3>
        <div class="gallery">
            <span
                class="swatch"
                v-for="swatch in toYellowGreenArray"
                :key="swatch"
                :style="`background-color: ${swatch}`"
            ><span>{{ swatch }}</span></span>
        </div>
    </div>
  </section>
</template>

<script>
import chroma from "chroma-js";

export default {
  name: 'PaletteDisplay',
  props: {
      hex: String,
      focusHueInput: Function,
  },
  data() {
      return {
          steps: 5,
          darkArray: [],
          lightArray: [],
          toBlueArray: [],
          toRedArray: [],
          toYellowArray: [],
          toGreenArray: [],
          toOrangeArray: [],
          toPurpleArray: [],
          toBlueGreenArray: [],
          toBlueVioletArray: [],
          toRedOrangeArray: [],
          toRedVioletArray: [],
          toYellowOrangeArray: [],
          toYellowGreenArray: [],
          customArray: [],
          customMixColor: null,
      }
  },
  methods: {
    makePalettes(color) {
        this.makeDarkArray(color)
        this.makeLightArray(color)
        this.makeToBlueArray(color)
        this.makeToRedArray(color)
        this.makeToYellowArray(color)
        this.makeToGreenArray(color)
        this.makeToOrangeArray(color)
        this.makeToPurpleArray(color)
        this.makeToBlueGreenArray(color)
        this.makeToBlueVioletArray(color)
        this.makeToRedOrangeArray(color)
        this.makeToRedVioletArray(color)
        this.makeToYellowOrangeArray(color)
        this.makeToYellowGreenArray(color)
    },
    makeDarkArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.darkArray[i] = chromaColor.darken(i)
        }
    },
    makeLightArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.lightArray[i] = chromaColor.brighten(i)
        }

        this.lightArray.reverse()
        this.lightArray.pop()
    },
    makeToBlueArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toBlueArray[i] = chroma.mix(chromaColor, 'blue', i * 0.25)
        }
    },
    makeToRedArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toRedArray[i] = chroma.mix(chromaColor, 'red', i * 0.25)
        }
    },
    makeToYellowArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toYellowArray[i] = chroma.mix(chromaColor, 'yellow', i * 0.25)
        }
    },
    makeToGreenArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toGreenArray[i] = chroma.mix(chromaColor, 'green', i * 0.25)
        }
    },
    makeToOrangeArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toOrangeArray[i] = chroma.mix(chromaColor, 'orange', i * 0.25)
        }
    },
    makeToPurpleArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toPurpleArray[i] = chroma.mix(chromaColor, 'purple', i * 0.25)
        }
    },
    makeToBlueGreenArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toBlueGreenArray[i] = chroma.mix(chromaColor, '#088f8f', i * 0.25)
        }
    },
    makeToBlueVioletArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toBlueVioletArray[i] = chroma.mix(chromaColor, 'BlueViolet', i * 0.25)
        }
    },
    makeToRedOrangeArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toRedOrangeArray[i] = chroma.mix(chromaColor, '#FF4433', i * 0.25)
        }
    },
    makeToRedVioletArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toRedVioletArray[i] = chroma.mix(chromaColor, '#A50055', i * 0.25)
        }
    },
    makeToYellowOrangeArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toYellowOrangeArray[i] = chroma.mix(chromaColor, '#FFAE42', i * 0.25)
        }
    },
    makeToYellowGreenArray(color) {
        const chromaColor = chroma(color)

        for (var i = 0; i < this.steps; i++) {
            this.toYellowGreenArray[i] = chroma.mix(chromaColor, 'Yellowgreen', i * 0.25)
        }
    },
    handleCustomColorInput(e) {
        this.customMixColor = e.target.value;
    },
    makeCustomMixArray(color1, color2) {
        const chromaColor = color2 ? chroma(color2) : null
        const otherColor = color1 ? chroma(color1) : null

        if (!otherColor) {
            return
        }
        
        for (var i = 0; i < this.steps; i++) {
            this.customArray[i] = chroma.mix(chromaColor, otherColor, i * 0.25)
        }
    },
    clearCustomColor() {
        this.customMixColor = null
    },
  },
  mounted() {
      if (this.hex) this.makePalettes(this.hex)

      document.getElementById('custom-color-input').value = chroma.random()
  },
  watch: {
    hex() {
        this.makePalettes(this.hex)

        if (this.customMixColor) {
            this.makeCustomMixArray(this.customMixColor, this.hex)
        }
    },
    customMixColor() {
        if (this.customMixColor !== 'undefined') {
            this.makeCustomMixArray(this.customMixColor, this.hex)
        }
    },
  },
}
</script>

<style>
.palette-stage {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 0 10rem 0;
}

.gallery-wrapper {
    margin: 10px 20px;
}

.gallery {
    display: flex;
    margin: 0 auto;
    max-width: 800px;
    justify-content: center;
}

.swatch {
    width: 100px;
    height: 100px;
    transition: all 0.2;
    box-shadow: none;
}

.swatch:hover {
    transform: scale(1.1);
    transition: all 0.2s;
    box-shadow: var(----boxShad);
}

.swatch span {
    display: block;
    background-color: var(--darkGrey);
    color: white;
    margin-top: 100%;
}

.gallery-wrapper.custom {
    background-color: var(--transBg);
    color: white;
    margin-top: 2rem;
    padding: 0 0 2rem 0;
    width: 80%;
    border-radius: var(--borRad);
    position: relative;
}

.gallery-wrapper.light-to-dark {
    width: 80%;
    margin-top: 2rem;
}

.light-to-dark .swatch:nth-child(5) {
    transform: scale(1.15);
    margin-top: -5px;
    transition: all 0.5s;
    box-shadow: var(--boxShad);
}

.light-to-dark .swatch:nth-child(5):hover {
    transform: scale(1.35);
}

.gallery-wrapper.custom .capture-color input {
    width: var(--customMixInputWidth);
}

.gallery-wrapper.custom .color-captured input {
    width: var(--customMixInputWidth);
}

.change-color {
    margin-top: 2rem;
}

.change-color button {
    position: absolute;
    right: 2rem;
    top: 1rem;
}

.romance {
    font-size: 180%;
}

.romance .selected {
    height: var(--customMixCircDim);
    width: var(--customMixCircDim);
    border-radius: 50%;
    margin-right: 10px;
    margin-bottom: -15px;
    background-color: var(--primary);
    display: inline-block;
}

.romance .and {
    margin: 0 10px;
}
</style>
