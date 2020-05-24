<template>
  <div id="app">
    <div v-if="name && time">
      <d-minus :time="time" />
      <span>before</span>
      <h2>{{ name }}</h2>
    </div>
    <div v-else>
      <event-form />
    </div>
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
