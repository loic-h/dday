<template>
  <div @scroll="onScrollHandler" ref="list" :class="{ smooth: isSmooth }">
    <ul ref="inner">
      <li v-if="!loop" />
      <li
        v-for="(v, i) in displayedRange"
        :key="i"
        :class="{
          item: true,
          active: value === v
        }"
        @click="onClick"
        ref="items">
        <span>
          {{ v }}
        </span>
      </li>
      <li v-if="loop" />
    </ul>
  </div>
</template>

<script>
const DELTA = 20;

export default {
  props: {
    value: String,
    range: Array,
    loop: { type: Boolean, default: true }
  },
  data() {
    return {
      delta: 0,
      scrollTop: null,
      displayedRange: this.range,
      isSmooth: false
    }
  },
  mounted() {
    const index = this.range.findIndex(x => x === this.value);
    this.goTo(this.$refs.items[index], null);
    this.$nextTick(() => {
      this.updateRange();
    });

    this.isScrollEnded = checkIsScrollEnded(this.onScrollEnd);
  },
  methods: {
    onClick(e) {
      const li = e.target.closest('li');
      this.goTo(li, true);
    },
    onScrollHandler(e) {
      this.isScrollEnded();
      this.$nextTick(this.onScroll)
    },
    onScroll() {
      this.updateRange();
      let index = Math.round(this.$refs.list.scrollTop / this.itemHeight());
      if (this.loop) {
        index ++;
      }
      const value = this.displayedRange[index];
      const scrollTop = this.$refs.list.scrollTop;
      this.$emit('input', value);
    },
    onScrollEnd() {
      let index = Math.round(this.$refs.list.scrollTop / this.itemHeight());
      if (this.loop) {
        index ++;
      }
      const item = this.$refs.items[index];
      this.goTo(item, false);
    },
    goTo(el, smooth) {
      this.$refs.list.scrollTo({
        top: el.previousSibling.offsetTop,
        behavior: smooth ? 'smooth' : 'auto'
      });
    },
    updateRange() {
      if (!this.loop) {
        return;
      }
      const scrollTop = this.$refs.list.scrollTop;
      const delta = this.itemHeight() * ( DELTA - 1 );
      if (scrollTop < delta) {
        const bottomIndex = this.range.findIndex(x => x == this.displayedRange[0]) || this.range.length;
        let bottomArray = this.range.slice(0, bottomIndex);
        while(bottomArray.length < DELTA) {
          bottomArray = this.range.concat(bottomArray);
        }
        bottomArray = bottomArray.slice(bottomArray.length - DELTA)
        this.displayedRange = [
          ...bottomArray,
          ...this.displayedRange
        ]
        this.$nextTick(() => {
          this.$refs.list.scrollTop = this.$refs.list.scrollTop + bottomArray.length * this.itemHeight()
        })
      } else if (this.$refs.inner.clientHeight - scrollTop < delta) {
        const topIndex = this.range.findIndex(x => x == this.displayedRange[this.displayedRange.length - 1]);
        let topArray = this.range.slice(topIndex + 1);
        while(topArray.length < DELTA) {
          topArray = topArray.concat(this.range);
        }
        topArray = topArray.slice(0, DELTA)
        this.displayedRange = [
          ...this.displayedRange,
          ...topArray,
        ]
      }
    },
    itemHeight() {
      return this.$refs.items && this.$refs.items[0].clientHeight;
    }
  }
};

const checkIsScrollEnded = cb => {
  let timer;
  return function() {
    timer && clearTimeout(timer);
    timer = setTimeout(cb, 200);
  };
};
</script>

<style scoped>
div {
  height: 3em;
  overflow: auto;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.smooth {
  scroll-behavior: smooth;
}

div::-webkit-scrollbar {
  display: none;
}

ul {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

li {
  height: 1em;
  position: relative;
  padding: 0 0.3em;
  display: flex;
  align-items: center;
}

li:not(.active) {
  opacity: 0.5;
}

li span {
  position: relative;
}

li:not(.active) span {
  font-size: 0.75em;
}

li.active span {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

</style>
