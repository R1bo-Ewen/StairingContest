<script>
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
  
  </script>
  
  <main>
    <div>
      <p><button type="button" on:click >Start Starring</button></p>
      
      <video style="height:200px; width: 300px;" alt = "A video of you." autoplay muted></video>
    </div>
  </main>
  
  <style>
    .logo {
      height: 6em;
      padding: 1.5em;
      will-change: filter;
    }
    .logo:hover {
      filter: drop-shadow(0 0 2em #646cffaa);
    }
    .logo.svelte:hover {
      filter: drop-shadow(0 0 2em #ff3e00aa);
    }
    .read-the-docs {
      color: #888;
    }
  </style>