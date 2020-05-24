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
    value: String
  },
  computed: {
    formattedValue() {
      return this.monthInLetter(this.value)
    }
  },
  created() {
    this.range = moment.monthsShort();
  },
  methods: {
    onInput(value) {
      this.$emit('input', this.monthInNumbers(value));
    },
    monthInLetter(value) {
      return moment().month(parseInt(value) - 1).format('MMM');
    },
    monthInNumbers(value) {
      return moment().month(value).format('MM');
    },
  }
};
</script>
