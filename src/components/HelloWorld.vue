<script setup lang="ts">
import { onMounted, Ref, ref } from 'vue'
import * as THREE from 'three'
import { Camera } from 'three';

const CANV = ref<HTMLCanvasElement | undefined>()

const vec = (x: number, y: number, z: number) => new THREE.Vector3(x, y, z)

const init = () => {
  const camera = new THREE.OrthographicCamera(3,3,2,2, 0,10000)
  camera.position.set(0, 10, 0)
  camera.lookAt(0, 0, 0)

  const scene = new THREE.Scene()
  scene.add(new THREE.PointLight())
  scene.add(new THREE.Mesh(
    new THREE.SphereGeometry(1,1,1),
    new THREE.MeshStandardMaterial()
  ))

  const [width, height] = [600, 400]
  const renderer = new THREE.WebGLRenderer({antialias: false})
  renderer.setSize(width, height)
  renderer.setPixelRatio(width /  height)
  return {
    renderer, camera, scene
  }
}

onMounted(() => {
  const {renderer, camera, scene} = init()

  CANV.value!.appendChild(renderer.domElement)
  

  const tick = () => {
    renderer.render(scene, camera)
    requestAnimationFrame(tick)
    
  }

  tick()
})

</script>

<template>

  <canvas ref="CANV"> </canvas>
  
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
