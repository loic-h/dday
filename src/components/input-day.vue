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
      return [...Array(days).keys()].map(x => `${x}`);
    },
    formattedValue() {
      return `${parseInt(this.value)}`;
    }
  },
  watch: {
    value: {
      handler(value) {
        if(this.range.indexOf(value) < 0) {
          this.$emit('input', '01');
        }
      },
      immediate: true
    },
    range: {
      handler(value) {
        if(value.indexOf(this.value) < 0) {
          this.$emit('input', '01');
        }
      },
      immediate: true
    }
  },
  methods: {
    onInput(value) {
      this.$emit('input', value);
    }
  }
};
</script>
