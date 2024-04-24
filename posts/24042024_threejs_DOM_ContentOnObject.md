---
title: Week 7 Homework 
published_at: 2024-04-24
snippet: Threejs example - DOM-contentOnObject
disable_html_sanitization: true
---

<div id="three_container"></div>

<canvas></canvas>

<script type="module">
import * as THREE from "/scripts/threejs/three.js";
import { OrbitControls } from "/scripts/threejs/OrbitControls.js";

const BOX_SIZE = 150;
let FONT_SIZE = 5;
const PADDING = 5;

//the three-container div
const container = document.getElementById('three_container')
const w = container.parentNode.scrollWidth
const h = w * 9 / 16

document.documentElement.style.setProperty("--box-size", BOX_SIZE + "px");
document.documentElement.style.setProperty("--font-size", FONT_SIZE + "px");
document.documentElement.style.setProperty("--padding", PADDING + "px");
const div = document.querySelector("div");
div.innerText = `This is DOM content. Font size: ${FONT_SIZE}px`;

var mesh, mesh2, renderer, scene, camera, controls, ctx;
var rotationY = 0;
var cameraZ = 20;
var perspective = 800;
var dpr = window.devicePixelRatio;
var screenPos = new THREE.Vector3

document.body.style.perspective = `${perspective}px`;

init();
animate();

function init() {
    // renderer
    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setPixelRatio(window.devicePixelRatio); // RESOLUTION
    renderer.setSize(w, h);
    // document.body.appendChild(renderer.domElement);
    container.appendChild(renderer.domElement);

    // scene
    scene = new THREE.Scene();

    // camera
    const fov =
        (180 * (2 * Math.atan(innerHeight / 2 / perspective))) / Math.PI;
    camera = new THREE.PerspectiveCamera(
        fov,
        window.innerWidth / window.innerHeight,
        1,
        10000
    );
    camera.position.set(0, 0, perspective);

    // controls
    controls = new OrbitControls(camera, renderer.domElement);

    // ambient
    scene.add(new THREE.AmbientLight(0xcccccc));

    // light
    var light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(0, 20, 0);
    scene.add(light);
    
    const canvas = document.createElement("canvas");
    ctx = canvas.getContext("2d");
    canvas.width = BOX_SIZE * dpr;
    canvas.height = BOX_SIZE * dpr;
    ctx.scale(dpr, dpr);
    canvas.style.width = BOX_SIZE + "px";
    canvas.style.height = BOX_SIZE + "px";

    const textTex = new THREE.CanvasTexture(canvas)
    const geom = new THREE.BoxBufferGeometry(BOX_SIZE, BOX_SIZE, 0.0001);
    const mat = new THREE.MeshBasicMaterial({ color: "skyblue", map: textTex });
    mesh = new THREE.Mesh(geom, mat);
    mesh.position.set(BOX_SIZE / 2, 0, 0);
    
    
    const geom2 = new THREE.BoxBufferGeometry(10, 10, 10);
    const mat2 = new THREE.MeshBasicMaterial({ color: "red" });
    mesh2 = new THREE.Mesh(geom2, mat2);
    mesh2.position.set(0, 0, 200);

    scene.add(mesh, camera, mesh2);
   
    
}

function updateFontSize(newSize) {
    document.documentElement.style.setProperty("--font-size", newSize + "px");

    const roundedFont = Math.floor(Math.round(FONT_SIZE * 100)) / 100;
    div.innerText = `This is DOM content. Font size: ${roundedFont}px`;
    
    ctx.fillStyle = "skyblue";
    ctx.fillRect(0, 0, BOX_SIZE, BOX_SIZE);
    ctx.fillStyle = "black";
    ctx.font = `${newSize}px serif`
    wrapText(
        ctx,
        `This is WebGL content. Font size: ${roundedFont}px`,
        PADDING,
        PADDING,
        BOX_SIZE - 2 * PADDING,
        FONT_SIZE
    );
    mesh.material.map.needsUpdate = true
}

function animate() {
    controls.update();

    // rotationY++
    div.style.transform =
        "translate3d(-50%, -50%, 0) rotateY(" + rotationY + "deg)";
    mesh.rotation.y = (rotationY / 180) * Math.PI;

    updateFontSize((FONT_SIZE += 0.03));

    // get screen-position of object, see https://stackoverflow.com/questions/11586527
    const widthHalf = window.innerWidth / 2
    const heightHalf = window.innerHeight / 2
    screenPos.copy(mesh2.position)
    screenPos.project(camera)
    screenPos.x = ( screenPos.x * widthHalf ) + widthHalf*0;
    screenPos.y = - ( screenPos.y * heightHalf ) + heightHalf*0;
    document.documentElement.style.setProperty("--left", screenPos.x + "px");
    document.documentElement.style.setProperty("--top", screenPos.y + "px");

    renderer.render(scene, camera);

    // setTimeout(() => requestAnimationFrame(animate), 1000);
    requestAnimationFrame(animate);
}

// From "HTML5 Canvas Text Wrap Tutorial", https://www.html5canvastutorials.com/tutorials/html5-canvas-wrap-text-tutorial
function wrapText(context, text, x, y, maxWidth, lineHeight) {
    var words = text.split(" ");
    var line = "";

    for (var n = 0; n < words.length; n++) {
        var testLine = line + words[n] + " ";
        var metrics = context.measureText(testLine);
        var testWidth = metrics.width;
        if (testWidth > maxWidth && n > 0) {
            context.fillText(line, x, y);
            line = words[n] + " ";
            y += lineHeight;
        } else {
            line = testLine;
        }
    }
    context.fillText(line, x, y);
}
</script>