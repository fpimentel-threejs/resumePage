<template>
  <Renderer ref="rendererC" antialias :orbit-ctrl="{ enabled: false }" resize="window">
    <Camera :position="{ x: camPos.camX, y: camPos.camY, z: camPos.camZ }" />
    <Scene ref="sceneRef" :background="0xffffff">
      <PointLight :intensity=".4" :position="{ x:-50, z: 50 }" />
      <AmbientLight :intensity=".8" />
      <GltfModel
          ref = "wheel"
          src="/assets/ONE-O.gltf"
          @load="onReady"
          @progress="onProgress"
          @error="onError"
          :position="{ x: -40 + camPos.moveMesh}"
      />

      <button v-if="started" class="btn btn-circle glass" id="startButton"
          @click="gsap.to(camPos, {moveMesh: 40, camX: 0, camY:-35, camZ:30, duration: .8}); started = !started"
      >START</button>

      <Text
          text="NE"
          font-src="/assets/Couture_Bold.json"
          align="center"
          :size="30"
          :height="8"
          :position="{ x: 10 + camPos.moveMesh, y: 0, z: 2 }"
      >
        <LambertMaterial :color="0xd6de3b"/>
      </Text>
    </Scene>
  </Renderer>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'

const camPos = ref({
  moveMesh: 0,
  camX: 0,
  camY: 0,
  camZ: 200
})

import { GridHelper } from 'three'
import { Camera, LambertMaterial, PointLight, Renderer, Scene } from 'troisjs'
const rendererC = ref()
const wheel = ref()
const sceneRef = ref()
const started = ref(true)

const gridHelper = new GridHelper( 1000, 400, 0xffffff, 0xe5e4e2 );

onMounted(() => {
  const renderer = rendererC.value
  const sceneR = sceneRef.value

  sceneR.add(gridHelper)
  gridHelper.rotation.x = 90 * Math.PI / 180
  gridHelper.position.z = -20

  renderer.onBeforeRender(() => {
  })

})
</script>

<style>

#startButton {
  position: absolute;
  top: 80%;
  left: 20%;
  z-index: 3;
}

body {
  margin: 0;
}
canvas {
  display: block;
}
</style>