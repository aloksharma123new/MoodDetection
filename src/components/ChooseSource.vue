<template>
  <div id="app">
  Choose Source
  <button id="Image" @click="image()">Image</button>
  <button id="Cam" @click="camera()">Cam</button>
  <div>
  
  <DetectMood v-if="test === 1">Hello</DetectMood>


  <DetectMoodNew v-if="test === 2"></DetectMoodNew>
</div>
</div>
</template>

<script setup>
import { ref } from 'vue';
import DetectMood from './DetectMoodNew.vue'
import DetectMoodNew from './DetectMoodNewCam.vue'
let source = ref('');
const test = ref(0);
//source.value = 'img';
function image() {
  source.value = 'img';
  test.value = 1;
 // console.log(source);
}
function camera() {
  source.value = 'cam';
  test.value = 2;
  //console.log(source);
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
