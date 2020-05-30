<template>
  <input-range
    :value="value"
    :range="range"
    :loop="false"
    @input="onInput" />
</template>

<script>
import InputRange from "./input-range";

const MIN = 1970;
const MAX = 2999;

export default {
  components: {
    InputRange
  },
  props: {
    value: String
  },
  created() {
    this.range = [...Array(MAX - MIN + 1).keys()].map(x => `${x + MIN}`);
  },
  methods: {
    onInput(value) {
      this.$emit("input", value);
    }
  }
};
</script>
