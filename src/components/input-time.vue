<template>
  <div class="input-time">
    <input type="text" :value="formattedTime" :placeholder="placeholder" @focus="onFocus" @blur="onBlur" />
    <div class="modal" v-if="isFocus">
      <div class="modal-inner">
        <div class="date">
          <input-day class="input" :value="day" :year="year" :month="month" @input="onDayInput" />
          <input-month class="input" :value="month" @input="onMonthInput" />
          <input-year class="input" :value="year" @input="onYearInput" />
        </div>
        <div class="time">
          <input-hour class="input" :value="hour" @input="onHourInput" />
          <input-minute class="input" :value="minute" @input="onMinuteInput" />
        </div>
      </div>
      <button @click="onValidation">ok</button>
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
    value: Number, // Unix
    placeholder: String
  },
  data() {
    return {
      isFocus: false,
      year: null,
      month: null,
      day: null,
      hour: null,
      minute: null,
      time: null
    }
  },
  computed: {
    moment() {
      return moment(this.value || Date.now());
    },
    formattedTime() {
      return this.value ? this.moment.format("D MMM YYYY, hh:mm") : '';
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
    onFocus() {
      this.isFocus = true;
    },
    onBlur() {
      // this.isFocus = false;
    },
    onValidation() {
      console.log("onValidation")
      this.isFocus = false;
      const time = moment(`${this.year}-${this.month}-${this.day} ${this.hour}:${this.minute}`).valueOf();
      this.$emit('input', time);
    }
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.modal-inner {
  display: flex;
  flex-direction: column;
  justify-content: center;
  max-width: 20rem;
}

.modal-inner > div {
  display: flex;
  justify-content: center;
}

.modal-inner > div:not(:last-child) {
  margin-bottom: 1em;
}

.input:not(:first-child) {
  margin-left: 0.5em;
}

.input:not(:last-child) {
  margin-right: 0.5em;
}

button {
  margin-top: 2em;
  font-size: 1.5rem;
}
</style>
