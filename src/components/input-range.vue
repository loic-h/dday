<template>
  <div @wheel="onScrollHandler" ref="list" :class="{ smooth: isSmooth }">
    <ul ref="inner"
      :style="{
        paddingTop: listPadding,
        paddingBottom: listPadding
      }">
      <li
        v-for="(v, i) in displayedRange"
        :key="i"
        :class="{
          item: true,
          active: value === v
        }"
        ref="items">
        <span>
          {{ v }}
        </span>
      </li>
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
  computed: {
    previous() {
      const index = this.loop && this.index <= 0 ? this.range.length - 1 : this.index - 1;
      return this.range[index];
    },
    next() {
      const index = this.loop && this.index >= this.range.length - 1 ? 0 : this.index + 1;
      return this.range[index];
    },
    listPadding() {
      return this.itemHeight() * 3;
    }
  },
  mounted() {
    const index = this.range.findIndex(x => x === this.value);
    this.$refs.list.scrollTop = this.$refs.items[index - 1].offsetTop;
    this.$nextTick(() => {
      this.updateRange();
    })

    this.isScrollEnded = checkIsScrollEnded(this.onScrollEnd);
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
    },
    onScrollHandler(e) {
      this.isScrollEnded();
      this.$nextTick(this.onScroll)
    },
    onScroll() {
      this.updateRange();
      const index = Math.round(this.$refs.list.scrollTop / this.$refs.items[0].clientHeight);
      const value = this.displayedRange[index + 1];
      const scrollTop = this.$refs.list.scrollTop;
      this.$emit('input', value);
    },
    onScrollEnd() {
      const scrollTop = Math.round(this.$refs.list.scrollTop / this.itemHeight()) * this.itemHeight()
      this.isSmooth = true;
      this.$nextTick(() => {
        this.$refs.list.scrollTop = scrollTop;
        this.$nextTick(() => {
          this.isSmooth = false;
        })
      })
    },
    updateRange() {
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
}

li:not(.active) {
  opacity: 0.5;
}

li:not(.active) span {
  font-size: 0.75em;
}

</style>
