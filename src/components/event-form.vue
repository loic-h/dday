<template>
  <div class="event-form">
    <div class="line">
      How long from
    </div>
    <div class="line">
      <input class="line" type="text" v-model="name" placeholder="event">
    </div>
    <div class="line">
      on
    </div>
    <div class="line">
      <input-time :value="time" @input="onTimeChange" placeholder="Date" />
    </div>
    <div class="line">
      <a :href="link" v-if="link">{{ link }}</a>
    </div>
  </div>
</template>

<script>
import InputTime from "./input-time";
import moment from 'moment';

export default {
  name: 'EventForm',
  components: {
    InputTime
  },
  data() {
    return {
      name: null,
      time: null
    };
  },
  computed: {
    link() {
      if (!this.name || !this.time) {
        return;
      }
      const url = new URL(window.location);
      url.searchParams.append('n', this.name);
      url.searchParams.append('t', this.time);
      return url.href;
    }
  },
  methods: {
    onTimeChange(value) {
      this.time = value
    }
  }
};
</script>

<style>
.event-form {
  display: flex;
  flex-direction: column;
  font-size: 2rem;
}

.line {
  margin-bottom: 1em;
}

input[type=text] {
  border-bottom: #454545 solid 1px;
  text-align: inherit;
  color: #222222;
}
</style>
