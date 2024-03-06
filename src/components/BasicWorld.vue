<template>
  <div class="cube-container">
    <a href="#" target="_self" rel="home"><button class="exit-btn" onclick='document.getElementById("bworld").remove();'> HOME </button></a>
    
    <div id="menuPanel">
      <button id="startButton">Click to Start</button>
    </div>
		<canvas id="bworld" class="canvas"></canvas>
  </div>
</template>

<script>
import * as THREE from '../../node_modules/three'
import { PointerLockControls } from '../../node_modules/three/examples/jsm/controls/PointerLockControls'
import Stats from '../../node_modules/three/examples/jsm/libs/stats.module'
export default {
  name: 'BasicWorld',
  props: {
    msg: String
  }
  ,mounted(){
    const scene = new THREE.Scene()
    scene.add(new THREE.AxesHelper(5))

    const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
    )
    camera.position.y = 1
    camera.position.z = 2

    const canvas = document.getElementById('bworld');
    const renderer = new THREE.WebGLRenderer({ antialias: true, canvas });
    renderer.setSize(window.innerWidth, window.innerHeight)
    document.body.appendChild(renderer.domElement)

    const menuPanel = document.getElementById('menuPanel')
    const startButton = document.getElementById('startButton') 
    startButton.addEventListener(
        'click',
        function () {
            controls.lock()
        },
        false
    )

    const controls = new PointerLockControls(camera, renderer.domElement)
    // controls.addEventListener('change', () => console.log("Controls Change"))
    controls.addEventListener('lock', () => menuPanel.style.display = 'none')
    controls.addEventListener('unlock', () => menuPanel.style.display = 'block')

    const planeGeometry = new THREE.PlaneGeometry(100, 100, 50, 50)
    const material = new THREE.MeshBasicMaterial({
        color: 0x00ff00,
        wireframe: true,
    })
    const plane = new THREE.Mesh(planeGeometry, material)
    plane.rotateX(-Math.PI / 2)
    scene.add(plane)

    const cubes = []
    for (let i = 0; i < 100; i++) {
        const geo = new THREE.BoxGeometry(
            Math.random() * 4,
            Math.random() * 16,
            Math.random() * 4
        )
        const mat = new THREE.MeshBasicMaterial({ wireframe: true })
        switch (i % 3) {
            case 0:
                mat.color = new THREE.Color(0xff0000)
                break
            case 1:
                mat.color = new THREE.Color(0xffff00)
                break
            case 2:
                mat.color = new THREE.Color(0x0000ff)
                break
        }
        const cube = new THREE.Mesh(geo, mat)
        cubes.push(cube)
    }
    cubes.forEach((c) => {
        c.position.x = Math.random() * 100 - 50
        c.position.z = Math.random() * 100 - 50
        c.geometry.computeBoundingBox()
        c.position.y =
            ((c.geometry.boundingBox ).max.y -
                (c.geometry.boundingBox ).min.y) /
            2
        scene.add(c)
    })

    const onKeyDown = function (event) {
        switch (event.code) {
            case "KeyW":
                controls.moveForward(.25)
                break
            case "KeyA":
                controls.moveRight(-.25)
                break
            case "KeyS":
                controls.moveForward(-.25)
                break
            case "KeyD":  
                controls.moveRight(.25)
                break
        }
    }
    document.addEventListener('keydown', onKeyDown, false)

    window.addEventListener('resize', onWindowResize, false)
    function onWindowResize() {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
        render()
    }


    const stats = new Stats()
    // document.body.appendChild(stats.dom)
    function animate() {
        requestAnimationFrame(animate)
        //controls.update()
        render()
        stats.update()
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

            #menuPanel {
                position: absolute;
                background-color: rgba(255, 255, 255, 0.5);
                top: 0px;
                left: 0px;
                width: 100%;
                height: 100%;
            }

            #startButton {
                height: 50px;
                width: 200px;
                margin: -25px -100px;
                position: relative;
                top: 50%;
                font-size: 32px;
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
