<template>
  <section id="main-tool" class="py-15">
    <div class="max-w-7xl mx-auto px-6">
      <div class="grid lg:grid-cols-12 gap-10">
        
        <!-- Input Panel -->
        <div class="lg:col-span-5 rounded-2xl p-10 border border-gray-300">
          <h2 class="text-3xl font-semibold tracking-tight mb-10">Atur Vektor</h2>
          
          <!-- Vektor A -->
          <div class="mb-12">
            <div class="flex items-center gap-3 mb-6">
              <div class="w-8 h-8 bg-emerald-500/10 text-emerald-400 rounded-2xl flex items-center justify-center font-bold">A</div>
              <h3 class="text-xl font-medium">Vektor A</h3>
            </div>
            
            <div class="grid grid-cols-3 gap-6">
              <div>
                <label class="block text-zinc-400 text-sm mb-2">X</label>
                <input v-model="vectorA.x" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" @change="dotProductCalc"/>
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Y</label>
                <input v-model="vectorA.y" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" @change="dotProductCalc"/>
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Z</label>
                <input v-model="vectorA.z" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" @change="dotProductCalc"/>
              </div>
            </div>
          </div>

          <!-- Vektor B -->
          <div>
            <div class="flex items-center gap-3 mb-6">
              <div class="w-8 h-8 bg-blue-500/10 text-blue-400 rounded-2xl flex items-center justify-center font-bold">B</div>
              <h3 class="text-xl font-medium">Vektor B</h3>
            </div>
            
            <div class="grid grid-cols-3 gap-6">
              <div>
                <label class="block text-zinc-400 text-sm mb-2">X</label>
                <input v-model="vectorB.x" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" @change="dotProductCalc">
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Y</label>
                <input v-model="vectorB.y" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" @change="dotProductCalc">
              </div>
              <div>
                <label class="block text-zinc-400 text-sm mb-2">Z</label>
                <input v-model="vectorB.z" type="number" step="0.1"
                  class="input-focus w-25 border border-gray-400 rounded-2xl px-2 py-3 text-2xl font-mono focus:outline-none" @change="dotProductCalc">
              </div>
            </div>
          </div>

          <!-- Result -->
          <div class="mt-10 p-6 rounded-2xl border border-gray-400">
            <div class="flex justify-between items-center">
              <div>
                <div class="text-zinc-400 text-sm">A · B =</div>
                <div id="dotResult" class="text-4xl font-mono font-semibold text-black">8.0</div>
              </div>
              <div class="text-right">
                <div class="text-zinc-400 text-sm">Sudut θ</div>
                <div id="angleResult" class="text-4xl font-mono font-semibold text-black">38.2°</div>
              </div>
            </div>
          </div>
        </div>

        <!-- 3D Visualization -->
        <div class="lg:col-span-7">
          <div class="rounded-3xl overflow-hidden border border-gray-300 h-[680px] relative">
            <div id="three-container" class="w-full h-full"></div>
            
            <!-- Overlay Info -->
            <div class="absolute top-8 left-8 bg-white backdrop-blur-md px-6 py-3 rounded-2xl border border-gray-300">
              <p class="text-xs text-gray-700">Drag untuk memutar • Scroll untuk zoom</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, onBeforeMount, ref, watch } from 'vue'
import * as THREE from 'three'

const vectorA = ref({x:'', y:'', z:''})
const vectorB = ref({x:'', y:'', z:''})

const threeContainer = ref(null)
const dotProduct = ref(0)
const sudut = ref(0)
const highlight = ref(false)

let scene, camera, renderer
let lineA, coneA, lineB, coneB

const dotProductCalc = () => {
  const a = vectorA.value
  const b = vectorB.value

  // perkalian dot product
  dotProduct.value = a.x * b.x + a.y * b.y + a.z * b.z

  const magA = Math.sqrt(a.x * a.x + a.y * a.y + a.z * a.z)
  const magB = Math.sqrt(b.x * b.x + b.y * b.y + b.z * b.z)
  
  if(magA === 0 || magB === 0){
    sudut.value = 0
    return
  }

  // kalkulasi arah kosinius dengan mencari theta nya
  let theta = dotProduct.value / (magA * magB)
  theta = Math.max(-1, Math.min(1, theta))

  sudut.value = Math.acos(theta) * 180 / Math.PI

  console.table({
    dotProduct: dotProduct.value,
    magA: magA,
    magB: magB,
    theta: theta,
    sudut: sudut.value
  })
}

</script>