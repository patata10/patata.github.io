<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Chamonix Driving Simulator</title>
<style>
  body { margin:0; overflow:hidden; background:black; }
  canvas { display:block; }
</style>
</head>
<body>

<script src="https://cdn.jsdelivr.net/npm/three@0.152.2/build/three.min.js"></script>

<script>
let scene = new THREE.Scene();
let camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
let renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// carretera simple
let roadGeometry = new THREE.PlaneGeometry(2000, 10);
let roadMaterial = new THREE.MeshBasicMaterial({color:0x333333});
let road = new THREE.Mesh(roadGeometry, roadMaterial);
road.rotation.x = -Math.PI/2;
scene.add(road);

// "edificis" bàsics
for(let i=0;i<200;i++){
  let geo = new THREE.BoxGeometry(5, Math.random()*20+5, 5);
  let mat = new THREE.MeshBasicMaterial({color:0x888888});
  let b = new THREE.Mesh(geo, mat);
  b.position.set((Math.random()-0.5)*100, geo.parameters.height/2, (Math.random()-0.5)*1000);
  scene.add(b);
}

// cotxe (camera)
let speed = 0;
let direction = 0;

camera.position.set(0,2,0);

document.addEventListener("keydown", e=>{
  if(e.key==="ArrowUp") speed=0.5;
  if(e.key==="ArrowDown") speed=-0.3;
  if(e.key==="ArrowLeft") direction=0.03;
  if(e.key==="ArrowRight") direction=-0.03;
});

document.addEventListener("keyup", ()=>{
  speed=0;
  direction=0;
});

function animate(){
  requestAnimationFrame(animate);

  camera.rotation.y += direction;
  camera.position.x += Math.sin(camera.rotation.y)*speed;
  camera.position.z += Math.cos(camera.rotation.y)*speed;

  renderer.render(scene, camera);
}
animate();
</script>

</body>
</html>
