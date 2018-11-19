<template>
  <div id="app">
    <DrawingBoard/>
  </div>
</template>

<script>
import DrawingBoard from './components/DrawingBoard.vue'

export default {
  name: 'app',
  components: {
    DrawingBoard
  }
}
</script>

<style>
body, #app {
  text-align: center;
  margin: 0;
  padding: 0;
  background: #f1f1f1;
  overflow: hidden;
}
</style>