<template>
  <div class="cube-container">
    <a href="#" target="_self" rel="home"><button class="exit-btn" onclick='document.getElementById("scc").remove();'> HOME </button></a>
    
    <div id="menuPanel">
      <button id="startButton">Click to Start</button>
    </div>
		<canvas id="scc" class="canvas"></canvas>
  </div>
</template>

<script>
import * as THREE from '../../node_modules/three'
import { PointerLockControls } from '../../node_modules/three/examples/jsm/controls/PointerLockControls'
export default {
  name: 'SCC',
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
    camera.position.x = 10

    const canvas = document.getElementById('scc');
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
    controls.addEventListener('lock', () => menuPanel.style.display = 'none')
    controls.addEventListener('unlock', () => menuPanel.style.display = 'block')

    const planeGeometry = new THREE.PlaneGeometry(100, 100, 50, 50)
    const material = new THREE.MeshBasicMaterial({
        color: 0x00ff00,
    })
    const plane = new THREE.Mesh(planeGeometry, material)
    plane.rotateX(-Math.PI / 2)
    scene.add(plane)

    const color = 0xED10CD;
    const intensity = 3;
    const light = new THREE.DirectionalLight(color, intensity);
    light.position.set(-1, 2, 4);
    scene.add(light);
    
    const cubes = []
        const geo = new THREE.BoxGeometry(
          10,
          12,
          3
        )
        const mat = new THREE.MeshPhongMaterial({ 
          wireframe: true,
          color : 0xED10CD
        })
        mat.color = new THREE.Color(0xff0000)
        const cube = new THREE.Mesh(geo, mat)
        cubes.push(cube)
        cubes.forEach((c) => {
          c.position.x = 28
          c.position.z = 6
          c.position.y = 6
          c.geometry.computeBoundingBox()
          scene.add(c)
          console.log(c.geometry.boundingBox)
        })

      const onKeyDown = function (event) {
        let possiblePosition = controls
        switch (event.code){
          case "KeyW":
            if (!isColliding(new THREE.Vector3(possiblePosition.getObject().position.x, 1, possiblePosition.getObject().position.z), possiblePosition.getObject().position, 1))  {
              controls.moveForward(.15)
            }
            break
          case "KeyA":
            if (!isColliding(new THREE.Vector3(possiblePosition.getObject().position.x, 1, possiblePosition.getObject().position.z), possiblePosition.getObject().position, 1)) {
              controls.moveRight(-.15)
            }
            break
          case "KeyS":
            if (!isColliding(new THREE.Vector3(possiblePosition.getObject().position.x, 1, possiblePosition.getObject().position.z), possiblePosition.getObject().position, 1))  {
              controls.moveForward(-.15)
            }
            break
          case "KeyD":  
            if (!isColliding(new THREE.Vector3(possiblePosition.getObject().position.x, 1, possiblePosition.getObject().position.z), possiblePosition.getObject().position, 1))  {
              controls.moveRight(.15)
            }
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
    function isColliding(direction, position, distance) {
      let v = false
      for ( let i = 0, l = cubes.length; i < l; i ++ ) {
        let oneCube = cubes[i]
        let minX = oneCube.position.x + oneCube.geometry.boundingBox.min.x - 0.5
        let maxX = oneCube.position.x + oneCube.geometry.boundingBox.max.x + 0.5
        let minZ = oneCube.position.z + oneCube.geometry.boundingBox.min.z - 0.5
        let maxZ = oneCube.position.z + oneCube.geometry.boundingBox.max.z + 0.5
        let minY = oneCube.position.y + oneCube.geometry.boundingBox.min.y - 0.5
        let maxY = oneCube.position.y + oneCube.geometry.boundingBox.max.y + 0.5
        if(  (position.x > minX && position.x < maxX) && (position.z > minZ && position.z < maxZ)){
          v = true
        }
      }
      return v
    }
    


    function animate() {
        requestAnimationFrame(animate)
        render()
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
