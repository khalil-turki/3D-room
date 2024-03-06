<template>
  <div class="cube-container">
		<canvas id="cube" class="canvas"></canvas>
    <a href="#" target="_self" rel="home"><button class="exit-btn" onclick='document.getElementById("cube").remove();'> HOME </button></a>
  </div>
</template>

<script>
import * as THREE from '../../node_modules/three/build/three.module.js';
import { OrbitControls } from '../../node_modules/three/examples/jsm/controls/OrbitControls'
import Stats from '../../node_modules/three/examples/jsm/libs/stats.module'
export default {
  name: 'Cube',
  props: {
    msg: String
  }
  ,
  mounted() {
    const scene = new THREE.Scene()
    const canvas = document.getElementById('cube');
    scene.add(new THREE.AxesHelper(5))
    const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
    )
    camera.position.z = 2
    // const renderer = new THREE.WebGLRenderer()
    const renderer = new THREE.WebGLRenderer({ antialias: true, canvas });

    renderer.setSize(window.innerWidth, window.innerHeight)
    document.body.appendChild(renderer.domElement)
    const controls = new OrbitControls(camera, renderer.domElement)
    const geometry = new THREE.BoxGeometry()
    const material = new THREE.MeshPhongMaterial({
        color: 0xDC143C
    })
    const color = 0xED10CD;
    const intensity = 3;
    const light = new THREE.DirectionalLight(color, intensity);
    light.position.set(-1, 2, 4);
    scene.add(light);
    const cube = new THREE.Mesh(geometry, material)
    scene.add(cube)
    window.addEventListener('resize', onWindowResize, false)
    function onWindowResize() {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
        render()
    }
    // const stats = new Stats()
    // document.body.appendChild(stats.dom)
    function animate() {
        requestAnimationFrame(animate)
        cube.rotation.x += 0.005;
        cube.rotation.y += 0.01;
        render()
        // stats.update()
    }
    function render() {
        renderer.render(scene, camera)
    }
    animate()
  }
}
</script>

<style scoped>

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.canvas{
  width: 100%;
  height: 100%;
}
.exit-btn{
  position: absolute;
  z-index: 1;
  background-color: rgba(255, 255, 255, 0.5);
  outline:2px solid rgba(255, 255, 255, 0.247);
  color:rgb(255, 255, 255);
  font-weight: 900;
  border-radius: 10px;
  left:30px;
  top:30px;
  padding:10px 20px;
  cursor:pointer;
}
.exit-btn:hover{
  outline:2px solid rgba(255, 255, 255, 0.8);
  transition-duration: 0.3s;
}
.exit-btn:active{
  outline:2px solid rgb(255, 255, 255);
  box-shadow: 0px 0px 10px 4px white;
  transition-duration: 0.1s;
}
</style>
