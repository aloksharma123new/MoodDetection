<template>
  <!--<img alt="Vue logo" src="./assets/logo.png">-->
  <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
 <div>
  <ChooseSource/>
  
</div>
  
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import ChooseSource from './components/ChooseSource.vue'
//import DetectMood from './components/DetectMoodNew.vue'
//import DetectMoodNew from './components/DetectMoodNewCam.vue'
//import { ref } from 'vue';


export default {
  name: 'App',
  components: {
    ChooseSource,
  },
  
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
