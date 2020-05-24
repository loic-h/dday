<template>
  <div class="event-form">
    <span>How long until</span>
    <input type="text" v-model="name" placeholder="event">
    <span>on</span>
    <input-time :value="time" @input="onTimeChange" placeholder="Date" />
    <a :href="link" v-if="link">{{ link }}</a>
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

<style scoped>
.event-form {
  display: flex;
  flex-direction: column;
}
</style>
