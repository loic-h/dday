<template>
  <ul>
    <li @click="onPreviousClick">{{ previous }}</li>
    <li>{{ value }}</li>
    <li @click="onNextClick">{{ next }}</li>
  </ul>
</template>

<script>
export default {
  props: {
    value: String,
    range: Array,
    loop: { type: Boolean, default: true }
  },
  computed: {
    index() {
      return this.range.findIndex(x => x === this.value);
    },
    previous() {
      const index = this.loop && this.index <= 0 ? this.range.length : this.index;
      return this.range[index - 1];
    },
    next() {
      const index = this.loop && this.index >= this.range.length - 1 ? 0 : this.index;
      return this.range[index + 1];
    }
  },
  methods: {
    onPreviousClick() {
      if (typeof this.previous === 'undefined') {
        return;
      }
      this.$emit('input', this.previous);
    },
    onNextClick() {
      if (typeof this.next === 'undefined') {
        return;
      }
      this.$emit('input', this.next)
    }
  }
};
</script>
