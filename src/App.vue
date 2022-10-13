<template>
  <div id="app">
    <div class="container">
      <div class="form-group mt-5">
        <label for="txtBox"> </label>
      </div>
      <Keyboard></Keyboard>
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
</style>
