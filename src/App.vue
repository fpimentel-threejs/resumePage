<template>

  <!--HTML scene-->
  <div id="head" class="z-20 overflow-y-auto">
    <div id="title" class="text-emerald-200 leading-none">Fernando Pimentel</div>
    <div id="headline" class="text-slate-200 leading-normal">Junior frontend engineer and creative developer with a specialization in 3D web design.</div>
  </div>

  <div id="body" class="z-40">
    <div id="sectionTitle" class="text-slate-200 leading-normal">Education</div>
    <div class="" id="experience">
      <div class="p-10 bg-emerald-300/5 rounded-3xl mb-10">
        <div class="">
          <div>
            <div class="text-amber-100 text-4xl leading-normal" >McNeese State University</div>
            <div class="text-2xl">2017-2021</div>
          </div>
          <div id="description">
            <div class="text-3xl my-6">Computer Science Major w/ Philosphy Minor</div>
          </div>
        </div>
          <ul class="mt-10" id="list" style="list-style-type:disc;">
            <li class="text-xl leading-normal">3.4 GPA</li><br/>
            <li class="text-xl leading-normal">Member and leadership position holder of various student organizations including: Association of Computing and Machinery, Robotics Club, Theta Chi Theta Rho</li>
          </ul>
      </div>
    </div>

    <!--work experience-->
    <div id="sectionTitle" class="text-slate-200 leading-normal">Work Experience</div>
    <div class="" id="experience">
      <div class="p-10 bg-emerald-300/5 rounded-3xl mb-10">
        <div class="">
          <div>
            <div class="text-amber-100 text-4xl leading-normal" >Freelance Web Development</div>
            <div class="text-2xl">March 2022-Present</div>
          </div>
          <div id="description">
            <div class="text-3xl my-6">Creative Web Developer</div>
          </div>
        </div>
        <ul id="list" class="mt-10" style="list-style-type:disc;">
          <li class="text-xl leading-normal">create interactive 3D websites complete with stock and custom assets</li><br/>
          <li class="text-xl leading-normal">creation of 3D and 2D assets such as 3D models, brand logos, and skyboxes</li><br/>
          <li class="text-xl leading-normal">mount sites for testing and th assurance of customer satisfaction</li><br/>
          <li class="text-xl leading-normal">interact and discuss with clients to meet client needs to the fullest extent</li>
        </ul>
      </div>
      <div class="p-10 bg-emerald-300/5 rounded-3xl mb-10">
        <div class="" id="job">
          <div><a class="text-amber-100 text-4xl leading-normal" href="https://www.capitalone.com/">Capital One</a></div>
          <div class="text-2xl">June 2022-Present</div>
        </div>
        <div id="description">
          <div class="text-3xl my-6">Associate Banker</div>
          <ul id="list" class="mt-10" style="list-style-type:disc;">
            <li class="text-xl leading-normal">interact with various banking software to assure regulations are followed and customer needs are met</li><br/>
            <li class="text-xl leading-normal">open and manage personal and business accounts for customers</li><br/>
            <li class="text-xl leading-normal">complete customer transactions</li>
          </ul>
        </div>
      </div>
    </div>
    <div id="footer" class="font-bold m-8">You can follow me on <a class="link link-warning" href="https://www.linkedin.com/in/fpimentel/">LinkedIn</a> and <a class="link link-warning" href="https://www.instagram.com/rxnando/">Instagram</a>. Here's my <a class="link link-warning" href="https://github.com/fpimentel-threejs/">Github</a> and <a class="link link-warning" href="mailto: fernandopimenton@gmail.com">Email</a>.</div>
  </div>

  <!--3D scene-->
  <Renderer class="z-30" :alpha="true" ref="rendererC" antialias :orbit-ctrl="{ enabled: false }" resize="window">
    <Camera :position="{z: 30}" />
    <Scene ref="sceneRef">
      <AmbientLight :intensity="10"/>

      <Group ref="orbit1">
        <Sphere ref=sphere1 :radius="1" :widthSegments="64" :heightSegments="32" :position="{x: 2, y: 5}">
        </Sphere>
        <Tetrahedron ref=sphere2 :detail="0" :radius="1" :position="{x: -4, y: 2}">
        </Tetrahedron>
        <Torus ref=sphere3 :radius="1.5" :tube=".3" :tubularSegments="3" :position="{x: 6, y: 2}">
        </Torus>
        <Tetrahedron ref=sphere7 :radius="5" :detail="0" :position="{x: 15, y: 15}">
        </Tetrahedron>
      </Group>
      <Group ref="orbit2">
        <Torus ref=sphere4 :radius="2.5" :tube=".1" :tubularSegments="8" :position="{x: 12, y: 10, z: -20}">
        </Torus>
        <Torus ref=sphere5 :radius="4" :tube=".08" :tubularSegments="5" :position="{x: 4, y: 4, z: -35}">
        </Torus>
        <Sphere ref=sphere6 :radius="3" :widthSegments="64" :heightSegments="32" :position="{x: 8, y: 14}">
        </Sphere>
      </Group>
    </Scene>
  </Renderer>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { GridHelper, ShaderMaterial } from 'three'
import { TorusKnot, Torus, Tetrahedron, AmbientLight, Sphere, Group, Camera, Renderer, Scene } from 'troisjs'

const _VS =`

varying vec3 v_Normal;

void main() {
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
  v_Normal = normal;
}
`;

const _FS = `

  varying vec3 v_Normal;

  void main(){
  gl_FragColor = vec4(v_Normal, 1.0);
}
`;

const rendererC = ref()
const sceneRef = ref()
const orbit1 = ref()
const orbit2 = ref()
const sphere1 = ref()
const sphere2 = ref()
const sphere3 = ref()
const sphere4 = ref()
const sphere5 = ref()
const sphere6 = ref()
const sphere7 = ref()

onMounted(() => {
  const renderer = rendererC.value
  const shader1 = sphere1.value.mesh
  const shader2 = sphere2.value.mesh
  const shader3 = sphere3.value.mesh
  const shader6 = sphere6.value.mesh
  const shader7 = sphere7.value.mesh
  const orbit = orbit1.value.group
  const outerOrbit = orbit2.value.group
  const scenery = sceneRef.value.scene

  const grid = new GridHelper(50, 20)

  grid.rotation.x = 90* Math.PI / 180
  //scenery.add(grid)

  shader1.material = new ShaderMaterial({
    uniforms: {},
    vertexShader: _VS,
    fragmentShader: _FS,
  });

  shader2.material = new ShaderMaterial({
    uniforms: {},
    vertexShader: _VS,
    fragmentShader: _FS,
  });

  shader6.material = new ShaderMaterial({
    uniforms: {},
    vertexShader: _VS,
    fragmentShader: _FS,
  });

  shader7.material = new ShaderMaterial({
    uniforms: {},
    vertexShader: _VS,
    fragmentShader: _FS,
  });

  let timer = 0

  renderer.onBeforeRender(() => {

    timer += 1

    orbit.rotation.z += .01
    outerOrbit.rotation.z -= .007

    shader1.rotation.y += .07
    shader1.rotation.x += .07
    shader1.position.z = Math.sin(timer/50)* 30 -20

    shader2.rotation.y += .07
    shader2.rotation.x += .07
    shader2.position.z = Math.sin(timer/30 + 1)* 30 -40

    shader6.rotation.y += .07
    shader6.rotation.x += .07
    shader6.position.z = Math.sin(timer/60 + 1)* 30 -20

    shader7.rotation.y += .01
    shader7.rotation.x += .01
    shader7.position.z = Math.sin(timer/120 + 1)* 25 -150

    shader3.tubularSegments += 1
  })

})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Dosis:wght@600&family=Heebo:wght@200;400;900&family=Sono:wght@200;400&family=Tilt+Neon&display=swap');
#head{
  position: absolute;
  top: 0;
  left: 0;
}
#title{
  font-family: 'Tilt Neon', cursive;
  font-size: 10vw;
  width: 60%;
  margin: 8vw;
  text-shadow: 2px 2px 10px;
}

#headline {
  font-family: 'Tilt Neon', cursive;
  font-weight: 400;
  width: 300px;
  margin: 20vw 0 8vw 8vw;
  font-size: 5vw;
  text-shadow: 1px 1px 8px;
}

#sectionTitle{
  font-family: 'Tilt Neon', cursive;
  font-weight: 400;
  margin: 20vw 0 8vw 8vw;
  font-size: 6vw;
  text-shadow: 1px 1px 8px;
}

#body{
  position: absolute;
  top: 0;
  left: 0;
  margin: 120vw 0 0 0;
}

#experience {
  padding: 5% 5%;
  margin: 3vw;
  font-family: 'Sono', sans-serif;
  font-weight: 200;
  backdrop-filter: blur(6px);
}

#list {
  margin-left: 10%;
  font-family: 'Heebo', sans-serif;
  font-weight: 200;
}

#description {
  font-family: 'Heebo', sans-serif;
}

#footer{
  font-family: 'Heebo', sans-serif;
}

body {
  margin: 0;
}
canvas {
  position: fixed;
}

</style>