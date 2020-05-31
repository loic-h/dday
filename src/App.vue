<template>
  <div id="app">
    <template v-if="time">
      <d-minus :time="time" />
      <template v-if="name">
        <span>from</span>
        <h2>{{ name }}</h2>
      </template>
    </template>
    <template v-else>
      <event-form />
    </template>
  </div>
</template>

<script>
import DMinus from "./components/d-minus";
import EventForm from './components/event-form.vue';

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
@import './css/reset.css';
@import './css/font.css';

#app {
  text-align: center;
  color: #454545;
  font-weight: 300;
  min-height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-top: 30vh;
  box-sizing: border-box;
  font-size: 2rem;
  padding: 1rem;
}
</style>
