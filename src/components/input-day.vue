<template>
  <input-range
    :value="formattedValue"
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
    value: String,
    month: String,
    year: String
  },
  computed: {
    range() {
      const days = moment(`${this.year}-${this.month}`, `YYYY-MM`).daysInMonth();
      return [...Array(days).keys()].map(x => `${x + 1}`);
    },
    formattedValue() {
      return `${parseInt(this.value)}`;
    }
  },
  methods: {
    onInput(value) {
      this.$emit('input', value);
    }
  }
};
</script>
