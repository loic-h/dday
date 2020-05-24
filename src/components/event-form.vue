<template>
  <div class="event-form">
    <d-minus :time="time" />
    <input-time :value="time" @input="onTimeChange" placeholder="Date" />
    <span>until</span>
    <input type="text" v-model="name" placeholder="event">
  </div>
</template>

<script>
import DMinus from "./d-minus";
import InputTime from "./input-time";
import moment from 'moment';

export default {
  name: 'EventForm',
  components: {
    InputTime,
    DMinus
  },
  data() {
    return {
      name: null,
      time: Date.now()
    };
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
