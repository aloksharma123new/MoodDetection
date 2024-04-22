<template>
    <!-- eslint-disable -->
     <div class="contentarea">
    <h1>
        Using Javascript to capture Photo
    </h1>
    <div class="camera">
      <!--  <video autoplay id="video">Video stream not available.</video>-->
        <video id="videoCam"></video>
    </div>
    <div><button id="startbutton" @click="takepicture()">Take photo</button></div>
    <button id="startBtn" @click="openCam()">Open Webcam</button>
    <button id="MoodDetection" @click="detectMood()">Detect Mood</button>
    
    <canvas id="canvas"></canvas>
    <div class="output">
        <img id="photo" alt="The screen capture will appear in this box."> 
    </div>
</div>
  </template>
  <script setup>
import { ref } from 'vue';
import axios from 'axios';
//import { convertColor } from 'naive-ui/es/color-picker/src/utils';
const fsref = ref('');
var data = '';
var width = 320; // We will scale the photo width to this
        var height = 0; // This will be computed based on the input stream
        height = width / (4 / 3);
        

        var video = null;
        var canvas = null;
        var photo = null;
        

function clearphoto() {
    var context = canvas.getContext('2d');
    context.fillStyle = "#AAA";
    context.fillRect(0, 0, canvas.width, canvas.height);

    var data = canvas.toDataURL('image/png');
    photo.setAttribute('src', data);
}
function takepicture() {
    video = document.getElementById('videoCam');
    canvas = document.getElementById('canvas');
    photo = document.getElementById('photo');
    //startbutton = document.getElementById('startbutton');
    var context = canvas.getContext('2d');
    if (width && height) {        
        canvas.width = width;
        canvas.height = height;
        context.drawImage(video, 0, 0, width, height);        
         data = canvas.toDataURL('image/jpeg');        
        photo.setAttribute('src', data);  
        
        canvas.toBlob((blob) => {
    fsref.value = new File([blob], "fileName.jpg", { type: "image/jpeg" })
  console.log(fsref.value);
}, 'image/jpeg');

    } else {
        
        clearphoto();
    }
    //window.addEventListener('load', startup, false)
}

function openCam(){
         let All_mediaDevices=navigator.mediaDevices
         if (!All_mediaDevices || !All_mediaDevices.getUserMedia) {
            console.log("getUserMedia() not supported.");
            return;
         }
         All_mediaDevices.getUserMedia({
            audio: true,
            video: true
         })
         .then(function(vidStream) {
            let video = document.getElementById('videoCam');
            video.setAttribute('width', width);
                    video.setAttribute('height', height);

            if ("srcObject" in video) {
               video.srcObject = vidStream;
            } else {
               video.src = window.URL.createObjectURL(vidStream);
            }
            // eslint-disable-next-line
            video.onloadedmetadata = function(e) {
               video.play();
            };
         })
         .catch(function(e) {
            console.log(e.name + ": " + e.message);
         });
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
   /*mood = response.data
   console.log(mood.google.items[0].emotions);
   joy.value = mood.google.items[0].emotions.joy;
   sorrow.value = mood.google.items[0].emotions.sorrow;
   anger.value = mood.google.items[0].emotions.anger;
   surprise.value = mood.google.items[0].emotions.suprise;
   fear.value = mood.google.items[0].emotions.fear;
   console.log(joy);*/
 })
 .catch((error) => {
   console.error(error);
 });

}
  </script>
  <style>
  
    /* CSS comes here */
    #video {
        border: 1px solid black;
        width: 320px;
        height: 240px;
    }

    #photo {
        border: 1px solid black;
        width: 320px;
        height: 240px;
    }

    #canvas {
        display: none;
    }

    .camera {
        width: 340px;
        display: inline-block;
    }

    .output {
        width: 340px;
        display: inline-block;
    }

    #startbutton {
        display: block;
        position: relative;
        margin-left: auto;
        margin-right: auto;
        bottom: 36px;
        padding: 5px;
        background-color: #6a67ce;
        border: 1px solid rgba(255, 255, 255, 0.7);
        font-size: 14px;
        color: rgba(255, 255, 255, 1.0);
        cursor: pointer;
    }

    .contentarea {
        font-size: 16px;
        font-family: Arial;
        text-align: center;
    }
    

</style>
