<script setup lang="ts">
import { onMounted, Ref, ref } from "vue"
import * as THREE from "three"

import { FontLoader } from "three/examples/jsm/loaders/FontLoader.js"
import { TextGeometry } from "three/examples/jsm/geometries/TextGeometry.js"

const CANV = ref<HTMLDivElement | undefined>()

const vec = (x: number, y: number, z: number) => new THREE.Vector3(x, y, z)

const init = () => {
  const [width, height] = [900, 600]

  //const camera = new THREE.PerspectiveCamera(70, width / height, 1, 5000 )
  const camera = new THREE.OrthographicCamera(-width, width, height, -height)
  //camera.position.set(0, 10, 0)

  const scene = new THREE.Scene()

  {
    const light = new THREE.DirectionalLight(0xffffff)
    //light.lookAt(1, 0, 1)
    scene.add(light)
    scene.add(new THREE.AmbientLight(0xffffff, 0.5))
  }

  {
    const mesh = new THREE.Mesh(
      new THREE.PlaneGeometry(40, 40),
      new THREE.MeshStandardMaterial({ color: new THREE.Color(1, 0, 0) })
    )
    mesh.position.x = 20
    scene.add(mesh)
  }

  const renderer = new THREE.WebGLRenderer()

  let isMouseDown = false
  renderer.domElement.onmousedown = ev => {
    console.log(343)
    isMouseDown = true
  }

  renderer.domElement.onmouseup = ev => {
    console.log()
    isMouseDown = false
  }
  renderer.domElement.onmouseleave = ev => {
    isMouseDown = false
  }

  renderer.domElement.onmousemove = ev => {
    const {movementX, movementY} = ev
    if (isMouseDown) {
      camera.position.x += movementX
       camera.position.y += movementY
    }

  }

  renderer.setSize(width, height)
  //renderer.setPixelRatio(height / width)
  return {
    renderer,
    camera,
    scene,
    isMouseDown,
  }
}

onMounted(() => {
  const { renderer, camera, scene } = init()

  CANV.value!.appendChild(renderer.domElement)

  const loader = new FontLoader()

  loader.load("helvetiker_regular.typeface.json", function (font: any) {
    const geometry = new TextGeometry("Hello three.jsreereereereereereereeree!", {
      font: font,
      size: 50,
      height: 5,
      curveSegments: 5,
    })

    scene.add(new THREE.Mesh(geometry, new THREE.MeshStandardMaterial()))
  })

  let f = 0

  camera.position.set(0, 0, 10)
  camera.up.set(0, 1, 0)
  
    camera.lookAt(0, 0, 0)
  const tick = () => {
    const FF = f * 0.02

    camera.updateProjectionMatrix()
    renderer.render(scene, camera)
    requestAnimationFrame(tick)
    //camera.rotation.x =  180* 0.0174533
    

    f++
  }

  tick()
})
</script>

<template>
  <div ref="CANV"></div>
  d
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
