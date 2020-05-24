<template>
  <input-range
    :value="value"
    @input="onInput"
    :range="range" />
</template>

<script>
import moment from 'moment';
import InputRange from './input-range';

export default {
  components: {
    InputRange
  },
  props: {
    value: String
  },
  computed: {
    range() {
      return [...Array(24).keys()].map(x => `${x}`.padStart(2, 0));
    }
  },
  methods: {
    onInput(value) {
      this.$emit('input', value);
    }
  }
};
</script>
