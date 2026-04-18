<script setup>
import { onMounted, onBeforeMount, nextTick, ref, watch, computed } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

const vectorA = ref({x: 0, y: 0, z: 0})
const vectorB = ref({x: 0, y: 0, z: 0})

const threeContainer = ref(null)
const dotProduct = ref(0)
const angle = ref(0)

let scene = null
let camera = null
let renderer = null
let controls = null
let arrowA = null
let arrowB = null

const init = async () => {
  setTimeout(() => {
    const container = threeContainer.value
    if (!container) return
  
    scene = new THREE.Scene()
    scene.background = new THREE.Color(0xffffff)
  
    camera = new THREE.PerspectiveCamera(90, container.clientWidth / container.clientHeight, 0.1, 1000)
    camera.position.set(8, 4, 6)
  
    renderer = new THREE.WebGLRenderer({ antialias: true })
    renderer.setSize(container.clientWidth, container.clientHeight)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    container.appendChild(renderer.domElement)
  
    controls = new OrbitControls(camera, renderer.domElement)
    controls.enableDamping = true
    controls.dampingFactor = 0.08
  
    scene.add(new THREE.AxesHelper(7))
    scene.add(new THREE.GridHelper(2, 2, 0x64748b, 0x0efff))
  
    animate()
    updateAll()
  }, 100)                  
}

const updateAll = () => {
  if (!scene) return

  const vecA = new THREE.Vector3(
    Number(vectorA.value.x) || 0,
    Number(vectorA.value.y) || 0,
    Number(vectorA.value.z) || 0
  )
  const vecB = new THREE.Vector3(
    Number(vectorB.value.x) || 0,
    Number(vectorB.value.y) || 0,
    Number(vectorB.value.z) || 0
  )

  const dot = vecA.dot(vecB)
  const magA = vecA.length()
  const magB = vecB.length()

  dotProduct.value = dot.toFixed(2)

  if (magA === 0 || magB === 0) {
    angle.value = 0
  } else {
    let cosTheta = dot / (magA * magB)
    cosTheta = Math.max(-1, Math.min(1, cosTheta))
    angle.value = (Math.acos(cosTheta) * 180 / Math.PI).toFixed(1)
  }

  if (!arrowA) {
    arrowA = new THREE.ArrowHelper(
      vecA.clone().normalize(),
      new THREE.Vector3(0, 0, 0),
      Math.max(magA, 1),
      0x10b981,
      0.4,
      0.25
    )
    scene.add(arrowA)
  } else {
    arrowA.setDirection(vecA.clone().normalize())
    arrowA.setLength(Math.max(magA, 1))
  }

  if (!arrowB) {
    arrowB = new THREE.ArrowHelper(
      vecB.clone().normalize(),
      new THREE.Vector3(0, 0, 0),
      Math.max(magB, 1),
      0x3b82f6,
      0.4,
      0.25
    )
    scene.add(arrowB)
  } else {
    arrowB.setDirection(vecB.clone().normalize())
    arrowB.setLength(Math.max(magB, 1))
  }
}

const animate = () => {
  requestAnimationFrame(animate)
  if (controls) controls.update()
  if (renderer && scene && camera) renderer.render(scene, camera)
}

const handleResize = () => {
  if (!camera || !renderer || !threeContainer.value) return
  camera.aspect = threeContainer.value.clientWidth / threeContainer.value.clientHeight
  camera.updateProjectionMatrix()
  renderer.setSize(threeContainer.value.clientWidth, threeContainer.value.clientHeight)
}

watch([vectorA, vectorB], updateAll, { deep: true })

onMounted(() => {
  init()
  window.addEventListener('resize', handleResize)
})

onBeforeMount(() => {
  if (renderer) renderer.dispose()
  window.removeEventListener('resize', handleResize)
})
</script>

<template>
  <section id="history" class="pt-13 pb-5">
    <div class="text-center mb-7">
      <h1 class="text-3xl font-bold mb-2">Sejarah</h1>
      <span class="text-zinc-400">Telusuri perjalanan evolusi vektor dari konsep awal hingga pengembangan modern</span>
    </div>
    <div class="max-w-7xl mx-auto px-6">
      <div class="grid lg:grid-cols-12 gap-10">
        
        <!-- figure -->
        <div class="hidden sm:block lg:col-span-5 py-6">
          <img src="/public/josiah.png" class="w-full h-auto josiah" alt="">
        </div>

        <!-- description -->
        <div class="lg:col-span-7">
          <!-- <h1 class="text-2xl font-semibold text-center">Josiah Willard Gibbs</h1> -->
          <p class="text-justify md:text-lg sm:text-xs mt-5">Josiah Willard Gibbs (1839–1903) adalah seorang ilmuwan Amerika yang dikenal sebagai <i>"fathers of thermodynamics"</i> dan <i>"father of vector analysis"</i>. Ia memperkenalkan konsep vektor dan operasi vektor seperti dot product dan cross product dalam karyanya <i>"Elementa de Vector"</i> pada tahun 1881. Karyanya ini menjadi fondasi matematika modern untuk ilmu fisika dan teknik.</p>
          
          <p class="text-justify md:text-lg sm:text-xs mt-4">Vektor dot product adalah operasi aljabar yang menghasilkan sebuah skalar dengan mengalikan magnitude dua vektor dan kosinus sudut di antara keduanya. Secara grafis, ia merepresentasikan proyeksi satu vektor ke arah vektor lainnya; jika hasilnya positif maka kedua vektor searah, jika nol maka keduanya tegak lurus (ortogonal), dan jika negatif maka keduanya berlawanan arah. Dalam pengembangan web dan grafis komputer, konsep ini sangat krusial untuk menghitung intensitas cahaya (shading), deteksi tabrakan, hingga menentukan orientasi objek dalam ruang 2D atau 3D. </p>

          <p class="text-justify md:text-lg sm:text-xs mt-4">Dalam dunia pemrograman modern, dot product digunakan untuk menghitung sudut antara dua vektor, memeriksa apakah vektor saling tegak lurus, serta menghitung proyeksi satu vektor ke arah lainnya. Konsep ini juga menjadi dasar dari algoritma machine learning, khususnya dalam perhitungan similarity antara data.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="main-tool" class="py-15">
    <div class="text-center mb-7">
      <h1 class="text-3xl font-bold mb-2">Visualisasi</h1>
      <span class="text-zinc-400">Visualisasi vektor 3D dengan sudut antara vektor</span>
    </div>
    <div class="max-w-7xl mx-auto px-6">
      <div class="grid lg:grid-cols-12 sm:grid-cols-2 gap-7">
        
        <!-- Input Panel -->
        <div class="lg:col-span-5 rounded-2xl p-10 border border-gray-300">
          <h2 class="text-2xl font-semibold tracking-tight mb-10">Input Vektor</h2>
          
          <!-- Vektor A -->
          <div class="mb-12">
            <div class="flex items-center gap-3 mb-6">
              <div class="w-8 h-8 bg-emerald-500/10 text-emerald-400 rounded-2xl flex items-center justify-center font-bold">A</div>
              <h3 class="text-md font-medium">Vektor A</h3>
            </div>
            
            <div class="grid grid-cols-3 gap-6">
              <div>
                <label class="block text-zinc-400 text-sm mb-2">X</label>
                <input v-model.number="vectorA.x" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" />
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Y</label>
                <input v-model.number="vectorA.y" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" />
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Z</label>
                <input v-model.number="vectorA.z" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" />
              </div>
            </div>
          </div>

          <!-- Vektor B -->
          <div>
            <div class="flex items-center gap-3 mb-6">
              <div class="w-8 h-8 bg-blue-500/10 text-blue-400 rounded-2xl flex items-center justify-center font-bold">B</div>
              <h3 class="text-md font-medium">Vektor B</h3>
            </div>
            
            <div class="grid grid-cols-3 gap-6">
              <div>
                <label class="block text-zinc-400 text-sm mb-2">X</label>
                <input v-model.number="vectorB.x" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" />
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Y</label>
                <input v-model.number="vectorB.y" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" />
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Z</label>
                <input v-model.number="vectorB.z" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" />
              </div>
            </div>
          </div>

          <!-- Result -->
          <div class="mt-10 p-6 rounded-2xl border border-gray-400">
            <div class="flex justify-between items-center">
              <div>
                <div class="text-zinc-400 text-sm">A · B =</div>
                <div class="text-4xl font-mono font-semibold text-black">{{ dotProduct }}</div>
              </div>
              <div class="text-right">
                <div class="text-zinc-400 text-sm">Sudut θ</div>
                <div class="text-4xl font-mono font-semibold text-black">{{ angle }}°</div>
              </div>
            </div>
          </div>
        </div>

        <!-- 3D Visualization -->
        <div class="lg:col-span-7">
          <div class="rounded-3xl overflow-hidden border border-gray-300 h-[680px] relative">
            <div id="three-container" class="w-full h-full" ref="threeContainer"></div>
            
            <!-- Overlay Info -->
            <div class="absolute top-8 left-8 bg-white backdrop-blur-md px-6 py-3 rounded-2xl border border-gray-300">
              <p class="text-xs text-gray-700">Drag untuk memutar • Scroll untuk zoom</p>
            </div>
          </div>
        </div>
      </div>

      <div class="grid lg:gird-cols-12 sm:grid-cols-1 border border-gray-300 p-2 rounded-2xl mt-6">
        <div class="bg-white backdrop-blur-md px-3 py-4">
          <span class="fas fa-square-root-variable text-xs font-semibold"></span><span class="text-xs ms-2 text-gray-700">Contoh penulisan matematis</span>
          <div v-html="mathExplained"></div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.josiah {
  filter: grayscale(100%);
  transform: scaleX(-1);
}
</style>