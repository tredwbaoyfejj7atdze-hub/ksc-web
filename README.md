<!DOCTYPE html><html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KSC | GLOBAL MODE</title><link rel="manifest" href="manifest.json"><style>
body{
  margin:0;
  font-family:Arial;
  background:#05050a;
  color:white;
}

header{
  text-align:center;
  padding:20px;
  font-size:30px;
  background:#0d0d15;
}

.hero{
  text-align:center;
  padding:100px 20px;
}

.hero h1{
  font-size:70px;
  text-shadow:0 0 25px #a855f7;
}

.album-cover{
  width:260px;
  height:260px;
  margin:30px auto;
  background:url('https://via.placeholder.com/300') center/cover;
  border-radius:20px;
  box-shadow:0 0 40px #a855f7;
}

canvas{
  width:100%;
  height:120px;
}

.player{
  width:320px;
  margin:20px auto;
  background:#111;
  padding:15px;
  border-radius:10px;
}

button{
  background:#a855f7;
  border:none;
  padding:10px;
  color:white;
  cursor:pointer;
}

.progress{width:100%;}

.discography{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
  gap:20px;
  padding:40px;
}

.album{background:#111;padding:10px;border-radius:10px;cursor:pointer;}
.album:hover{box-shadow:0 0 20px #a855f7;}
.album img{width:100%;border-radius:10px;}

footer{text-align:center;padding:20px;background:#0d0d15;}
</style></head><body><header>KSC</header><section class="hero">
  <h1>KSC</h1>
  <p>GLOBAL ARTIST MODE</p>  <div class="album-cover" id="cover"></div><canvas id="visualizer"></canvas>

  <div class="player">
    <audio id="audio"></audio>
    <button onclick="playPause()">Play / Pause</button>
    <input type="range" id="progress" class="progress" value="0">
  </div>
</section><section>
  <h2 style="text-align:center;">Discografía</h2>
  <div class="discography"><div class="album" onclick="loadTrack('track1.mp3','https://via.placeholder.com/300')">
  <img src="https://via.placeholder.com/200">
  <h3>Track 1</h3>
</div>

<div class="album" onclick="loadTrack('track2.mp3','https://via.placeholder.com/300')">
  <img src="https://via.placeholder.com/200">
  <h3>Track 2</h3>
</div>

<div class="album" onclick="loadTrack('track3.mp3','https://via.placeholder.com/300')">
  <img src="https://via.placeholder.com/200">
  <h3>Track 3</h3>
</div>

  </div>
</section><footer>© 2026 KSC - GLOBAL</footer><script>
let audio=document.getElementById('audio');
let progress=document.getElementById('progress');
let cover=document.getElementById('cover');

function loadTrack(src,img){
  audio.src=src;
  cover.style.background=`url(${img}) center/cover`;
  audio.play();
  initVisualizer();
}

function playPause(){
  if(audio.paused) audio.play(); else audio.pause();
}

audio.addEventListener('timeupdate',()=>{
  progress.value=(audio.currentTime/audio.duration)*100;
});

progress.addEventListener('input',()=>{
  audio.currentTime=(progress.value/100)*audio.duration;
});

function initVisualizer(){
  const canvas=document.getElementById('visualizer');
  const ctx=canvas.getContext('2d');
  const audioCtx=new (window.AudioContext||window.webkitAudioContext)();
  const src=audioCtx.createMediaElementSource(audio);
  const analyser=audioCtx.createAnalyser();

  src.connect(analyser);
  analyser.connect(audioCtx.destination);

  analyser.fftSize=256;
  const bufferLength=analyser.frequencyBinCount;
  const dataArray=new Uint8Array(bufferLength);

  function draw(){
    requestAnimationFrame(draw);
    analyser.getByteFrequencyData(dataArray);
    ctx.clearRect(0,0,canvas.width,canvas.height);
    let barWidth=(canvas.width/bufferLength)*2.5;
    let x=0;
    for(let i=0;i<bufferLength;i++){
      let h=dataArray[i];
      ctx.fillStyle=`hsl(${i*2},100%,50%)`;
      ctx.fillRect(x,canvas.height-h/2,barWidth,h);
      x+=barWidth+1;
    }
  }
  draw();
}

/* PWA */
if('serviceWorker' in navigator){
  navigator.serviceWorker.register('sw.js');
}
</script></body>
</html>
