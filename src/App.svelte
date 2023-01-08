<script lang='ts'>
	import { onMount } from 'svelte';
    import * as faceapi from 'face-api.js';

    let video;
    
    Promise.all([
        faceapi.nets.tinyFaceDetector.loadFromDisk('./models')
    ])
    
    const net = new faceapi.TinyFaceDetectorOptions()

    console.log(faceapi.nets)
	
    onMount(async () => {
        video.addEventListener('play', () =>{
            setInterval(async () => {
            const detections = await faceapi.detectSingleFace(video, net);
            if (detections){
                console.log(detections);
            }
            else{
                document.getElementById("Je_Parle").outerHTML = "Perdu !";
                setTimeout(GetRickRolled, 6000)
            }
            }, 100)
        })
	});

    function GetRickRolled(){
        document.location.href="https://www.youtube.com/watch?v=dQw4w9WgXcQ&ab_channel=RickAstley"; 
    }


    function getUserMedia(constraints) {
      // if Promise-based API is available, use it
        if (navigator.mediaDevices) {
            return navigator.mediaDevices.getUserMedia(constraints);
        }
        
      // otherwise try falling back to old, possibly prefixed API...
        var legacyApi = navigator.getUserMedia || navigator.webkitGetUserMedia ||
            navigator.mozGetUserMedia || navigator.msGetUserMedia;
            
        if (legacyApi) {
            // ...and promisify it
            return new Promise(function (resolve, reject) {
            legacyApi.bind(navigator)(constraints, resolve, reject);
            });
        }
    }
    function getStream (type) {
        if (!navigator.mediaDevices && !navigator.getUserMedia && !navigator.webkitGetUserMedia &&
            !navigator.mozGetUserMedia && !navigator.msGetUserMedia) {
            alert('User Media API not supported.');
            return;
    }

var constraints = {};
constraints[type] = true;
    
    getUserMedia(constraints)
      .then(function (stream) {
        var mediaControl = document.querySelector(type);
        
        if ('srcObject' in mediaControl) {
          mediaControl.srcObject = stream;
        } else if (navigator.mozGetUserMedia) {
          mediaControl.mozSrcObject = stream;
        } else {
          mediaControl.src = (window.URL || window.webkitURL).createObjectURL(stream);
        }
        
        mediaControl.play();
      })
      .catch(function (err) {
        alert('Error: ' + err);
      });
  }
  getStream('video')
  </script>
  
  <main>
    <div class="wrapper">
        <div class="box1">
            <img src = "../images/BG_stare.png" alt = "Me, looking at you.">
        </div>
        <div class="box2">
            <p id="Je_Parle">
                Moi contre toi, maintenant !
                Le premier qui arrête de regarder 
                l'autre à perdu et, crois moi, t'as
                pas envie de perdre !
            </p>
        </div>
        <div class="box3">
            <video id = "video" bind:this={video} autoplay muted></video>
        </div>
    </div>
  </main>
  
  <style>
    .wrapper{
        align-items : center;
        justify-content:center;
        display: grid;
        grid-template-columns: 20% 20% 20% 35%;
        grid-template-rows: 50% 50%;
    }

    .box1 {
    grid-column-start: 1;
    grid-column-end: 4;
    grid-row-start: 1;
    grid-row-end: 3;
    position: relative;
    }

    p{
        font-size: 30px;
        align-items : center;
    }

    video{
        height: 100%; 
        width: 100%;
    }

    img{
        height: 100%; 
        width: 100%;
    }
  </style>