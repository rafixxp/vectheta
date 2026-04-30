<script setup>
import { onMounted, onUnmounted } from 'vue'
import * as THREE from 'three'

let scene, camera, renderer, vectorA, vectorB, magnitudeLineA, magnitudeLineB
let time = 0

const initThreeJS = () => {
  const container = document.getElementById('hero-canvas')
  
  // Scene setup
  scene = new THREE.Scene()
  // scene.background = new THREE.Color(0x1a1a1a).convertSRGBToLinear().setAlpha(0.1) // Dark gray background with 10% opacity
  
  // Camera setup
  camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  )
  camera.position.z = 5
  
  // Renderer setup
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.setPixelRatio(window.devicePixelRatio)
  renderer.setClearColor(0x1a1a1a, 0.1) // Light gray with 10% opacity
  container.appendChild(renderer.domElement)
  
  // Create grid helper for reference
  const gridHelper = new THREE.GridHelper(40, 40, 0x444444, 0x222222)
  gridHelper.rotation.x = Math.PI / 2
  scene.add(gridHelper)
  
  // Create axes helper
  const axesHelper = new THREE.AxesHelper(3)
  scene.add(axesHelper)
  
  // Vector A (Red)
  const vectorAGeometry = new THREE.BufferGeometry()
  const vectorAVertices = new Float32Array([0, 0, 0, 2, 1, 0.5])
  vectorAGeometry.setAttribute('position', new THREE.BufferAttribute(vectorAVertices, 3))
  
  const vectorAMaterial = new THREE.LineBasicMaterial({ 
    color: 0xff0000, 
    linewidth: 3,
    transparent: true,
    opacity: 0.8
  })
  vectorA = new THREE.Line(vectorAGeometry, vectorAMaterial)
  scene.add(vectorA)
  
  // Vector B (Blue)
  const vectorBGeometry = new THREE.BufferGeometry()
  const vectorBVertices = new Float32Array([0, 0, 0, -1, 2, 1])
  vectorBGeometry.setAttribute('position', new THREE.BufferAttribute(vectorBVertices, 3))
  
  const vectorBMaterial = new THREE.LineBasicMaterial({ 
    color: 0x0000ff, 
    linewidth: 3,
    transparent: true,
    opacity: 0.8
  })
  vectorB = new THREE.Line(vectorBGeometry, vectorBMaterial)
  scene.add(vectorB)
  
  // Magnitude line A (dashed line showing magnitude)
  const magnitudeAGeometry = new THREE.BufferGeometry()
  const magnitudeAVertices = new Float32Array([0, 0, 0, 2, 1, 0.5])
  magnitudeAGeometry.setAttribute('position', new THREE.BufferAttribute(magnitudeAVertices, 3))
  
  const magnitudeAMaterial = new THREE.LineDashedMaterial({ 
    color: 0xff6666, 
    linewidth: 2,
    dashSize: 0.1,
    gapSize: 0.05,
    transparent: true,
    opacity: 0.6
  })
  magnitudeLineA = new THREE.Line(magnitudeAGeometry, magnitudeAMaterial)
  magnitudeLineA.computeLineDistances()
  scene.add(magnitudeLineA)
  
  // Magnitude line B (dashed line showing magnitude)
  const magnitudeBGeometry = new THREE.BufferGeometry()
  const magnitudeBVertices = new Float32Array([0, 0, 0, -1, 2, 1])
  magnitudeBGeometry.setAttribute('position', new THREE.BufferAttribute(magnitudeBVertices, 3))
  
  const magnitudeBMaterial = new THREE.LineDashedMaterial({ 
    color: 0x6666ff, 
    linewidth: 2,
    dashSize: 0.1,
    gapSize: 0.05,
    transparent: true,
    opacity: 0.6
  })
  magnitudeLineB = new THREE.Line(magnitudeBGeometry, magnitudeBMaterial)
  magnitudeLineB.computeLineDistances()
  scene.add(magnitudeLineB)
  
  // Add point spheres at vector endpoints
  const sphereGeometry = new THREE.SphereGeometry(0.1, 16, 16)
  
  const sphereAMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 })
  const sphereA = new THREE.Mesh(sphereGeometry, sphereAMaterial)
  sphereA.position.set(2, 1, 0.5)
  scene.add(sphereA)
  
  const sphereBMaterial = new THREE.MeshBasicMaterial({ color: 0x0000ff })
  const sphereB = new THREE.Mesh(sphereGeometry, sphereBMaterial)
  sphereB.position.set(-1, 2, 1)
  scene.add(sphereB)
  
  // Handle window resize
  const handleResize = () => {
    camera.aspect = window.innerWidth / window.innerHeight
    camera.updateProjectionMatrix()
    renderer.setSize(window.innerWidth, window.innerHeight)
  }
  
  window.addEventListener('resize', handleResize)
  
  // Animation loop
  const animate = () => {
    requestAnimationFrame(animate)
    
    time += 0.01
    
    // Animate vectors
    const vectorAEnd = new THREE.Vector3(
      Math.sin(time) * 2,
      Math.cos(time * 0.8) * 1.5,
      Math.sin(time * 1.2) * 0.8
    )
    
    const vectorBEnd = new THREE.Vector3(
      Math.cos(time * 0.7) * 1.8,
      Math.sin(time * 1.1) * 2,
      Math.cos(time * 0.9) * 1.2
    )
    
    // Update vector A
    vectorA.geometry.setAttribute('position', new THREE.BufferAttribute(new Float32Array([0, 0, 0, vectorAEnd.x, vectorAEnd.y, vectorAEnd.z]), 3))
    magnitudeLineA.geometry.setAttribute('position', new THREE.BufferAttribute(new Float32Array([0, 0, 0, vectorAEnd.x, vectorAEnd.y, vectorAEnd.z]), 3))
    magnitudeLineA.computeLineDistances()
    sphereA.position.copy(vectorAEnd)
    
    // Update vector B
    vectorB.geometry.setAttribute('position', new THREE.BufferAttribute(new Float32Array([0, 0, 0, vectorBEnd.x, vectorBEnd.y, vectorBEnd.z]), 3))
    magnitudeLineB.geometry.setAttribute('position', new THREE.BufferAttribute(new Float32Array([0, 0, 0, vectorBEnd.x, vectorBEnd.y, vectorBEnd.z]), 3))
    magnitudeLineB.computeLineDistances()
    sphereB.position.copy(vectorBEnd)
    
    // Rotate the entire scene slowly
    scene.rotation.y = Math.sin(time * 0.2) * 0.3
    scene.rotation.x = Math.cos(time * 0.15) * 0.2
    
    renderer.render(scene, camera)
  }
  
  animate()
  
  // Store cleanup function
  container.cleanup = () => {
    window.removeEventListener('resize', handleResize)
    if (container.contains(renderer.domElement)) {
      container.removeChild(renderer.domElement)
    }
    renderer.dispose()
    vectorAGeometry.dispose()
    vectorBGeometry.dispose()
    vectorAMaterial.dispose()
    vectorBMaterial.dispose()
  }
}

onMounted(() => {
  initThreeJS()
})

onUnmounted(() => {
  const container = document.getElementById('hero-canvas')
  if (container.cleanup) {
    container.cleanup()
  }
})
</script>

<template>
  <section class="min-h-screen relative flex items-center">
    <!-- Three.js Background -->
    <div id="hero-canvas" class="absolute inset-0 z-0"></div>

    <div class="max-w-5xl mx-auto px-6 relative z-9999 pt-2">
      <div class="text-center max-w-3xl mx-auto">
        <div class="inline-flex items-center gap-2 bg-dark-900/80 backdrop-blur-md border border-zinc-700 px-5 py-2 rounded-full text-sm mb-6">
          <span class="text-emerald-400">●</span>
          Pemodelan Dot Product Vektor 3D
        </div>
        
        <h1 class="text-6xl md:text-7xl font-semibold tracking-tighter leading-none mb-6">
          Kalkulasi.<br>
          Visualisasikan.<br>
          <span class="bg-gradient-to-r from-zinc-800 to-gray-500 bg-clip-text text-transparent">dan Mengerti.</span>
        </h1>
        
        <p class="text-xl text-gray-700 mb-10 max-w-lg mx-auto">
          Belajar dot product dengan cara yang bisa kamu lihat langsung.
        </p>

        <button onclick="document.getElementById('main-tool').scrollIntoView({ behavior: 'smooth' })" 
          class="px-10 py-5 bg-black text-white rounded-2xl font-medium text-lg hover:bg-zinc-200 transition flex items-center gap-3 mx-auto">
          Coba Sekarang 🚀
        </button>
      </div>
    </div>

    <!-- Scroll indicator -->
    <div class="absolute bottom-10 left-1/2 -translate-x-1/2 text-zinc-500 text-sm flex flex-col items-center gap-2">
      <span>Scroll untuk mulai</span>
      <i class="fas fa-chevron-down animate-bounce"></i>
    </div>
  </section>
</template>
