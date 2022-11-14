<script setup lang="ts">
import { onMounted, Ref, ref } from 'vue'
import * as THREE from 'three'

const CANV = ref<HTMLDivElement | undefined>()

const vec = (x: number, y: number, z: number) => new THREE.Vector3(x, y, z)

const init = () => {
  const [width, height] = [600, 400]
  
  const camera = new THREE.PerspectiveCamera(70, width / height, 1, 5000 )
  //const camera = new THREE.OrthographicCamera(-width, width, -height, height)
  //camera.position.set(0, 10, 0)
  

  const scene = new THREE.Scene()
  
  {
    const light = new THREE.DirectionalLight( 0xffffff );
    light.lookAt(1, 0, 1)
    scene.add(light);
    scene.add(new THREE.AmbientLight(0xffffff, 0.5))
  }

  {
    const mesh = new THREE.Mesh(
      new THREE.BoxGeometry(10, 10, 10),
      new THREE.MeshStandardMaterial({color: new THREE.Color(1, 0, 0)})
      )
    scene.add(mesh)
  }

  
  const renderer = new THREE.WebGLRenderer()
  renderer.setSize(width, height)
  //renderer.setPixelRatio(height / width)
  return {
    renderer, camera, scene
  }
}

onMounted(() => {
  const {renderer, camera, scene} = init()

  CANV.value!.appendChild(renderer.domElement)

  let f = 0
  const tick = () => {
    const FF = f* 0.02
    
    camera.updateProjectionMatrix()
    renderer.render(scene, camera)
    requestAnimationFrame(tick)
    
    camera.position.set(Math.cos(FF) * 10, Math.sin(FF) * 10, Math.sin(FF)* 10)
    camera.lookAt(0, 0, 0)

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
