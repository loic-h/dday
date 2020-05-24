<template>
  <div class="input-time">
    <!-- <input type="text" @focus="onFocus" @blur="onBlur" /> -->
    <div class="modal">
      <div class="date">
        <input-day :value="day" :year="year" :month="month" @input="onDayInput" />
        <input-month :value="month" @input="onMonthInput" />
        <input-year :value="year" @input="onYearInput" />
      </div>
      <div class="time">
        <input-hour :value="hour" @input="onHourInput" />
        <input-minute :value="minute" @input="onMinuteInput" />
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import InputDay from "./input-day";
import InputMonth from "./input-month";
import InputYear from "./input-year";
import InputHour from "./input-hour";
import InputMinute from "./input-minute";

export default {
  components: {
    InputDay,
    InputMonth,
    InputYear,
    InputHour,
    InputMinute
  },
  props: {
    value: Number // Unix
  },
  data() {
    return {
      // isFocus: false,
      year: null,
      month: null,
      day: null,
      hour: null,
      minute: null,
    }
  },
  computed: {
    moment() {
      return moment(this.value);
    }
  },
  watch: {
    value: {
      handler() {
        this.year = this.moment.format('YYYY');
        this.month = this.moment.format('MM');
        this.day = this.moment.format('DD');
        this.hour = this.moment.format('HH');
        this.minute = this.moment.format('mm');
      },
      immediate: true
    },
    year() {
      this.emitTime();
    },
    month() {
      this.emitTime();
    },
    day() {
      this.emitTime();
    },
    hour() {
      this.emitTime();
    },
    minute() {
      this.emitTime();
    }
  },
  methods: {
    onYearInput(value) {
      this.year = value;
    },
    onMonthInput(value) {
      this.month = value;
    },
    onDayInput(value) {
      this.day = value;
    },
    onHourInput(value) {
      this.hour = value;
    },
    onMinuteInput(value) {
      this.minute = value;
    },
    emitTime() {
      this.$emit('input', moment(`${this.year}-${this.month}-${this.day} ${this.hour}:${this.minute}`).valueOf());
    },
    onFocus() {
      this.isFocus = true;
    },
    onBlur() {
      this.isFocus = false;
    }
  }
}
</script>
