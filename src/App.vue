<template>
  <div id="app">
    <div class="container">
      <div class="form-group mt-5">
        <label for="txtBox"> </label>
      </div>
      <div class="keyboard">
        <Keyboard></Keyboard>
      </div>
    </div>
  </div>
</template>

<script>
import Keyboard from "./components/Keyboard.vue";

export default {
  components: { Keyboard },
  data() {
    return {};
  },
  methods: {

  },
};
</script>

<style scoped>
.container {
  max-width: 100%;

}

/* #app {
  background-color: #00ff80;

}

.keyboard {
  background-color: #00ff80;
} */
</style>
