<template>
  <div>
 <div class="first">
 <p><input type="file" accept="image/*" name="image" id="file" @change="loadFile2($event)"></p>
 <!--<p><button @click="loadFile($event)">submit</button></p>-->
 <p><img id="output" width="200" /></p>
 <!--<p>{{mood.google.items[0].emotions.joy}}</p>-->
 {{ joy }}
 {{ sorrow }}
 {{ anger }}
 {{ anger }}
{{ surprise }} 
{{ fear }} 
 
</div>

</div>

<div>
  <n-form
    ref="formRef"
    inline
    :label-width="80"
    :model="formValue"
    :rules="rules"
    :size="size"
  >
    <n-form-item label="Name" path="user.name">
      <n-input v-model:value="formValue.user.name" placeholder="Input Name" />
    </n-form-item>
    
  </n-form>

</div>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';
import FormInst from 'naive-ui';
const fsref = ref('');
let mood = '';
let joy = ref('');
let sorrow = ref('');
let anger = ref('');
let surprise = ref('');
let fear = ref('');


/*function loadFile(event) {
 console.log(document.getElementById("file").value);
var image = document.getElementById('file');
console.log(image);
console.log(event); 
}*/

function loadFile2(event) {
 console.log(document.getElementById("file").value);
 var image = document.getElementById('output');
   image.src = URL.createObjectURL(event.target.files[0]);
   fsref.value = event.target.files[0];
image.src = URL.createObjectURL(event.target.files[0]);
 detectMood();
}
async function detectMood() {
const form = new FormData();
form.append("providers", "google");
form.append("file", fsref.value);
form.append("fallback_providers", "");

const options = {
method: "POST",
url: "https://api.edenai.run/v2/image/face_detection",
headers: {
 Authorization: "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiZDEwM2MzMDMtYzJkMS00NzNlLWI5MTQtZGMxOTg1YWEyZDQ3IiwidHlwZSI6ImFwaV90b2tlbiJ9.Ieo7rAMgtICajVYTc3Zvsx0ZiJZmBGmx0GJs5rX0lsk",
 "Content-Type": "multipart/form-data"
},
data: form,
}
console.log(options);
axios
.request(options)
.then((response) => {
 console.log(response.data);
 mood = response.data
 console.log(mood.google.items[0].emotions);
 joy.value = mood.google.items[0].emotions.joy;
 sorrow.value = mood.google.items[0].emotions.sorrow;
 anger.value = mood.google.items[0].emotions.anger;
 surprise.value = mood.google.items[0].emotions.suprise;
 fear.value = mood.google.items[0].emotions.fear;
 console.log(joy);
})
.catch((error) => {
 console.error(error);
});

}
</script>