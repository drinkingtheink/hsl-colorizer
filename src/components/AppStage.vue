<template>
  <main>
    <section>
    <input type="color" @input="updateActiveColor" />
    </section>
    <section>
      <div v-show="activeColor" class="active-color-display" :style="activeColorDisplay" />
    </section>
  </main>
</template>

<script>
export default {
  name: 'AppStage',
  data() {
    return {
      activeColor: null,
    }
  },
  methods: {
    updateActiveColor(e) {
      this.activeColor = e.target.value;
    }
  },
  computed: {
    activeColorDisplay() {
      return `background: ${this.activeColor}`;
    }
  }
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
