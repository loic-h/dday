<template>
  <div id="app">
    <event-form />
  </div>
</template>

<script>
import EventForm from './components/event-form.vue'

export default {
  name: 'App',
  components: {
    EventForm
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
