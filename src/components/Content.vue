<script setup>
import { onMounted, onBeforeMount, nextTick, ref, watch, computed } from 'vue'
import katex from 'katex'
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
    scene.add(new THREE.GridHelper(4, 4, 0x64748b, 0x0efff))
  
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

  let magnitudoA = Math.sqrt(vectorA.value.x ** 2 + vectorA.value.y ** 2 + vectorA.value.z ** 2)
  let magnitudoB = Math.sqrt(vectorB.value.x ** 2 + vectorB.value.y ** 2 + vectorB.value.z ** 2)

  dotProduct.value = dot.toFixed(0)

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

const mathExplained = computed(() => {
const ax = parseFloat(vectorA.value.x)
const ay = parseFloat(vectorA.value.y)
const az = parseFloat(vectorA.value.z)

const bx = parseFloat(vectorB.value.x)
const by = parseFloat(vectorB.value.y)
const bz = parseFloat(vectorB.value.z)

const dot = (ax * bx) + (ay * by) + (az * bz)

const magA = Math.sqrt((ax ** 2) + (ay ** 2) + (az ** 2))
const magB = Math.sqrt((bx ** 2) + (by ** 2) + (bz ** 2))

const denominator = magA * magB

const cosTheta = dot / denominator

const theta = Math.acos(cosTheta) * (180 / Math.PI)

const formula = `
  \\begin{aligned}

  \\mathbf{a} \\cdot \\mathbf{b}
  &= (a_x \\cdot b_x) + (a_y \\cdot b_y) + (a_z \\cdot b_z) \\\\

  &= (${ax} \\cdot ${bx}) + (${ay} \\cdot ${by}) + (${az} \\cdot ${bz}) \\\\

  &= ${(ax * bx)} + ${(ay * by)} + ${(az * bz)} \\\\

  &= ${dot} \\\\[1.5em]

  |\\mathbf{a}|
  &= \\sqrt{a_x^2 + a_y^2 + a_z^2} \\\\

  &= \\sqrt{(${ax})^2 + (${ay})^2 + (${az})^2} \\\\

  &= \\sqrt{${ax ** 2} + ${ay ** 2} + ${az ** 2}} \\\\

  &= \\sqrt{${(ax ** 2) + (ay ** 2) + (az ** 2)}} \\\\

  &= ${magA.toFixed(4)} \\\\[1.5em]

  |\\mathbf{b}|
  &= \\sqrt{b_x^2 + b_y^2 + b_z^2} \\\\

  &= \\sqrt{(${bx})^2 + (${by})^2 + (${bz})^2} \\\\

  &= \\sqrt{${bx ** 2} + ${by ** 2} + ${bz ** 2}} \\\\

  &= \\sqrt{${(bx ** 2) + (by ** 2) + (bz ** 2)}} \\\\

  &= ${magB.toFixed(4)} \\\\[1.5em]

  \\cos \\theta
  &= \\frac{\\mathbf{a} \\cdot \\mathbf{b}}{|\\mathbf{a}||\\mathbf{b}|} \\\\[1em]

  &= \\frac{${dot}}{${magA.toFixed(4)} \\cdot ${magB.toFixed(4)}} \\\\[1em]

  &= \\frac{${dot}}{${denominator.toFixed(4)}} \\\\[1em]

  &= ${cosTheta.toFixed(4)} \\\\[1.5em]

  \\theta
  &= \\cos^{-1}(${cosTheta.toFixed(4)}) \\\\[1em]

  &= ${theta.toFixed(2)}^\\circ \\\\[1.5em]

  \\therefore
  \\quad
  \\theta \\approx ${theta.toFixed(2)}^\\circ

  \\end{aligned}
  `

  return katex.renderToString(formula, {
    displayMode: false,
    throwOnError: false,
  })
})

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
  <section id="history" class="py-20">
    <div class="max-w-6xl mx-auto px-6">
      <div class="text-center mb-12">
        <h1 class="text-4xl font-bold text-gray-900 mb-3">Sejarah</h1>
        <p class="text-gray-500 text-lg">Telusuri perjalanan evolusi vektor dari konsep awal hingga pengembangan modern</p>
      </div>
      
      <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
        <div class="grid md:grid-cols-2 gap-0">
          <div class="p-8 md:p-12 flex flex-col justify-center">
            <h2 class="text-2xl font-bold text-gray-900 mb-6">Josiah Willard Gibbs</h2>
            <p class="text-gray-600 leading-relaxed mb-4">Josiah Willard Gibbs (1839–1903) adalah seorang ilmuwan Amerika yang dikenal sebagai <em>"fathers of thermodynamics"</em> dan <em>"father of vector analysis"</em>. Ia memperkenalkan konsep vektor dan operasi vektor seperti dot product dan cross product dalam karyanya <em>"Elementa de Vector"</em> pada tahun 1881.</p>
            <p class="text-gray-600 leading-relaxed mb-4">Vektor dot product adalah operasi aljabar yang menghasilkan sebuah skalar dengan mengalikan magnitude dua vektor dan kosinus sudut di antara keduanya. Secara grafis, ia merepresentasikan proyeksi satu vektor ke arah vektor lainnya.</p>
            <p class="text-gray-600 leading-relaxed">Dalam dunia pemrograman modern, dot product digunakan untuk menghitung sudut antara dua vektor, memeriksa apakah vektor saling tegak lurus, serta menghitung proyeksi satu vektor ke arah lainnya.</p>
          </div>
          <div class="bg-gray-100 flex items-center justify-center p-8">
            <img src="/josiah.webp" class="w-full max-w-sm h-auto josiah rounded-2xl shadow-lg" alt="Josiah Willard Gibbs">
          </div>
        </div>
      </div>
    </div>
  </section>

  <section id="main-tool" class="py-24 bg-gradient-to-b from-white to-gray-50">
    <div class="max-w-6xl mx-auto px-6">
      <div class="text-center mb-16">
        <h1 class="text-4xl md:text-4xl font-bold text-gray-900 mb-4">Visualisasi</h1>
        <p class="text-gray-500 text-lg">Eksplorasi dot product dan sudut antara vektor dalam ruang 3D</p>
      </div>
      
      <div class="grid lg:grid-cols-12 gap-8 lg:gap-12">
        <!-- 3D Visualization -->
        <div class="lg:col-span-7 order-1 lg:order-1">
          <div class="bg-white rounded-2xl shadow-sm overflow-hidden h-[450px] sm:h-[550px] lg:h-[650px] border border-gray-100">
            <div id="three-container" class="w-full h-full" ref="threeContainer"></div>
          </div>
        </div>

        <!-- Input Panel -->
        <div class="lg:col-span-5 order-2 lg:order-2">
          <div class="space-y-6">
            <!-- Vektor A -->
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
              <div class="flex items-center gap-3 mb-5">
                <div class="w-8 h-8 bg-emerald-500 text-white rounded-lg flex items-center justify-center font-bold text-sm">A</div>
                <h3 class="font-semibold text-gray-900">Vektor A</h3>
              </div>
              <div class="grid grid-cols-3 gap-3">
                <div>
                  <label class="block text-gray-400 text-xs mb-2 font-medium">X</label>
                  <input v-model.number="vectorA.x" type="number" step="0.1"
                    class="w-full bg-gray-50 border-0 rounded-lg px-3 py-3 text-base font-mono focus:ring-2 focus:ring-emerald-500 focus:bg-white transition" />
                </div>
                <div>
                  <label class="block text-gray-400 text-xs mb-2 font-medium">Y</label>
                  <input v-model.number="vectorA.y" type="number" step="0.1"
                    class="w-full bg-gray-50 border-0 rounded-lg px-3 py-3 text-base font-mono focus:ring-2 focus:ring-emerald-500 focus:bg-white transition" />
                </div>
                <div>
                  <label class="block text-gray-400 text-xs mb-2 font-medium">Z</label>
                  <input v-model.number="vectorA.z" type="number" step="0.1"
                    class="w-full bg-gray-50 border-0 rounded-lg px-3 py-3 text-base font-mono focus:ring-2 focus:ring-emerald-500 focus:bg-white transition" />
                </div>
              </div>
            </div>

            <!-- Vektor B -->
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
              <div class="flex items-center gap-3 mb-5">
                <div class="w-8 h-8 bg-blue-500 text-white rounded-lg flex items-center justify-center font-bold text-sm">B</div>
                <h3 class="font-semibold text-gray-900">Vektor B</h3>
              </div>
              <div class="grid grid-cols-3 gap-3">
                <div>
                  <label class="block text-gray-400 text-xs mb-2 font-medium">X</label>
                  <input v-model.number="vectorB.x" type="number" step="0.1"
                    class="w-full bg-gray-50 border-0 rounded-lg px-3 py-3 text-base font-mono focus:ring-2 focus:ring-blue-500 focus:bg-white transition" />
                </div>
                <div>
                  <label class="block text-gray-400 text-xs mb-2 font-medium">Y</label>
                  <input v-model.number="vectorB.y" type="number" step="0.1"
                    class="w-full bg-gray-50 border-0 rounded-lg px-3 py-3 text-base font-mono focus:ring-2 focus:ring-blue-500 focus:bg-white transition" />
                </div>
                <div>
                  <label class="block text-gray-400 text-xs mb-2 font-medium">Z</label>
                  <input v-model.number="vectorB.z" type="number" step="0.1"
                    class="w-full bg-gray-50 border-0 rounded-lg px-3 py-3 text-base font-mono focus:ring-2 focus:ring-blue-500 focus:bg-white transition" />
                </div>
              </div>
            </div>

            <!-- Result -->
            <div class="bg-white rounded-2xl p-6 text-dark shadow-sm border border-gray-100">
              <div class="grid grid-cols-2 gap-6">
                <div>
                  <div class="text-gray-400 text-xs font-medium mb-2 uppercase tracking-wider">Dot Product</div>
                  <div class="text-4xl font-mono font-bold">{{ dotProduct }}</div>
                </div>
                <div class="text-right">
                  <div class="text-gray-400 text-xs font-medium mb-2 uppercase tracking-wider">Sudut θ</div>
                  <div class="text-4xl font-mono font-bold">{{ angle }}°</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Math Explanation -->
      <div class="mt-16 bg-white rounded-2xl p-8 shadow-sm border border-gray-100">
        <h3 class="text-xl font-semibold text-gray-900 mb-6">Penjelasan Matematis</h3>
        <div class="bg-gray-50 rounded-xl p-6 overflow-x-auto" v-html="mathExplained"></div>
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