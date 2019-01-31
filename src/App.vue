<template>
  <div id="app">
    <last-time :base-time="new Date()"></last-time>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import LastTime from './components/LastTime.vue';

@Component({
  components: {
    LastTime,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss"></style>
