---
title: Week 6 Homework (part 2)
published_at: 2024-04-17
snippet: three.js example - Hole Text
disable_html_sanitization: true
---

<div id="three_container"></div>

<!-- <canvas id="threejs"/> -->

<script id="three-script" type="module">
import * as THREE from '/scripts/threejs/three.js'
import {
  OrbitControls
} from '/scripts/threejs/OrbitControls.js'
import Stats from '/scripts/threejs/stats.module.js'
import {
  Font,
  FontLoader
} from '/scripts/threejs/FontLoader.js'
import {
  TextGeometry
} from '/scripts/threejs/TextGeometry.js'
import {
  CSG
} from '/scripts/threejs/csg.js'

//from lecture's code
const div = document.getElementById ("three_container")
const w = div.parentNode.scrollWidth
const h = w * 9 / 16

const scene = new THREE.Scene()
const camera = new THREE.PerspectiveCamera(75, 9 / 16, 0.1, 1000)
camera.position.set(3, 4, 5)
const renderer = new THREE.WebGLRenderer()
renderer.setSize(w, h)
// document.body.appendChild(renderer.domElement)
div.appendChild(renderer.domElement)

const controls = new OrbitControls(camera, renderer.domElement)
controls.enableDamping = true

const loader = new FontLoader()
loader.load('https://cdn.jsdelivr.net/gh/mrdoob/three.js@master/examples/fonts/helvetiker_bold.typeface.json', function(f) {
  const textGeometry = new TextGeometry('Hello', {
    font: f,
    size: 2,
    height: 0.2,
    curveSegments: 2
  })
  textGeometry.scale(1, 1, 3)
  textGeometry.center()

  const cubeCSG = CSG.fromGeometry(new THREE.BoxGeometry(10, 3, 0.25))
  const textCSG = CSG.fromGeometry(textGeometry)

  const subtractCSG = cubeCSG.subtract(textCSG)
  
  const finalMesh = CSG.toMesh(
    subtractCSG,
    new THREE.Matrix4()
  )
  finalMesh.material = new THREE.MeshNormalMaterial()
  scene.add(finalMesh)

})

window.addEventListener('resize', onWindowResize, false)

function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight
  camera.updateProjectionMatrix()
  renderer.setSize(window.innerWidth, window.innerHeight)
  render()
}

const stats = Stats()
document.body.appendChild(stats.dom)

function animate() {
  requestAnimationFrame(animate)
  controls.update()
  render()
  stats.update()
}

function render() {
  renderer.render(scene, camera)
}
animate()
</script>