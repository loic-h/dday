<template>
  <div id="app">
    <div v-if="time">
      <d-minus :time="time" />
      <template v-if="name">
        <span>from</span>
        <h2>{{ name }}</h2>
      </template>
    </div>
    <div v-else>
      <event-form />
    </div>
  </div>
</template>

<script>
import DMinus from "./components/d-minus";
import EventForm from './components/event-form.vue';

import './css/reset.css';
import './css/font.css';

export default {
  name: 'App',
  components: {
    EventForm,
    DMinus
  },
  data() {
    return {
      name: null,
      time: null
    }
  },
  mounted() {
    const params = (new URL(document.location)).searchParams;
    this.name = params.get('n');
    this.time = parseInt(params.get('t'));
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #454545;
  font-weight: 300;
  min-height: 100%;
  display: flex;
  flex-direction: column;
  padding-top: 30vh;
  box-sizing: border-box;
  font-size: 2rem;
}
</style>
