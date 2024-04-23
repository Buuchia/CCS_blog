---
title: Example three.js
published_at: 2024-04-17
snippet: three.js example - Hole Text
disable_html_sanitization: true
---
<!-- <script src="/scripts/threejs/three.module.min.js"></script> -->

<canvas id="threejs"/>

<script>
import * as THREE from '/scripts/threejs/three.cjs'
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

const scene = new THREE.Scene()
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(3, 4, 5)
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)

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