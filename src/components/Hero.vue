<script setup>
import { onMounted, onUnmounted } from 'vue'
import * as THREE from 'three'

let scene, camera, renderer
let arrows = []
let time = 0

const initThreeJS = () => {
  const container = document.getElementById('hero-canvas')
  
  scene = new THREE.Scene()
  
  camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  )
  camera.position.z = 15
  
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.setPixelRatio(window.devicePixelRatio)
  container.appendChild(renderer.domElement)
  
  const colors = [0xff6b6b, 0x4ecdc4, 0x45b7d1, 0x96ceb4, 0xffeaa7, 0xdfe6e9, 0xa29bfe, 0xfd79a8]
  const arrowCount = 200
  
  for (let i = 0; i < arrowCount; i++) {
    const group = new THREE.Group()
    
    const direction = new THREE.Vector3(
      Math.random() - 0.5,
      Math.random() - 0.5,
      Math.random() - 0.5
    ).normalize()
    
    const length = 1 + Math.random() * 2
    const end = direction.clone().multiplyScalar(length)
    
    const lineGeometry = new THREE.BufferGeometry()
    const vertices = new Float32Array([0, 0, 0, end.x, end.y, end.z])
    lineGeometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    
    const color = colors[Math.floor(Math.random() * colors.length)]
    const lineMaterial = new THREE.LineBasicMaterial({
      color: color,
      transparent: true,
      opacity: 0.6 + Math.random() * 0.4
    })
    
    const line = new THREE.Line(lineGeometry, lineMaterial)
    group.add(line)
    
    const coneGeometry = new THREE.ConeGeometry(0.08 + Math.random() * 0.1, 0.2 + Math.random() * 0.15, 8)
    const coneMaterial = new THREE.MeshBasicMaterial({ color: color, transparent: true, opacity: 0.8 })
    const cone = new THREE.Mesh(coneGeometry, coneMaterial)
    cone.position.copy(end)
    cone.lookAt(end.clone().multiplyScalar(1.5))
    group.add(cone)
    
    group.position.set(
      (Math.random() - 0.5) * 80,
      (Math.random() - 0.5) * 40,
      (Math.random() - 0.5) * 10 - 10
    )
    
    group.rotation.set(
      Math.random() * Math.PI,
      Math.random() * Math.PI,
      Math.random() * Math.PI
    )
    
    group.userData = {
      basePosition: group.position.clone(),
      baseRotation: group.rotation.clone(),
      speed: 0.3 + Math.random() * 0.7,
      amplitude: 0.5 + Math.random() * 1.5,
      phase: Math.random() * Math.PI * 2,
      rotationSpeed: 0.2 + Math.random() * 0.5
    }
    
    scene.add(group)
    arrows.push(group)
  }
  
  const handleResize = () => {
    camera.aspect = window.innerWidth / window.innerHeight
    camera.updateProjectionMatrix()
    renderer.setSize(window.innerWidth, window.innerHeight)
  }
  
  window.addEventListener('resize', handleResize)
  
  const animate = () => {
    requestAnimationFrame(animate)
    
    time += 0.01
    
    arrows.forEach((arrow) => {
      const data = arrow.userData
      
      arrow.position.x = data.basePosition.x + Math.sin(time * data.speed + data.phase) * data.amplitude
      arrow.position.y = data.basePosition.y + Math.cos(time * data.speed * 0.8 + data.phase) * data.amplitude
      arrow.position.z = data.basePosition.z + Math.sin(time * data.speed * 0.6 + data.phase) * data.amplitude * 0.5
      
      arrow.rotation.x = data.baseRotation.x + Math.sin(time * data.rotationSpeed + data.phase) * 0.5
      arrow.rotation.y = data.baseRotation.y + time * data.rotationSpeed * 0.3
      arrow.rotation.z = data.baseRotation.z + Math.cos(time * data.rotationSpeed + data.phase) * 0.5
    })
    
    scene.rotation.y = Math.sin(time * 0.05) * 0.1
    scene.rotation.x = Math.cos(time * 0.03) * 0.05
    
    renderer.render(scene, camera)
  }
  
  animate()
  
  container.cleanup = () => {
    window.removeEventListener('resize', handleResize)
    if (container.contains(renderer.domElement)) {
      container.removeChild(renderer.domElement)
    }
    renderer.dispose()
    arrows.forEach(arrow => {
      arrow.children.forEach(child => {
        if (child.geometry) child.geometry.dispose()
        if (child.material) child.material.dispose()
      })
    })
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
  <section class="h-screen overflow-hidden relative flex items-center justify-center bg-white">
    <div id="hero-canvas" class="absolute inset-0 z-0"></div>

    <div class="relative z-10 text-center px-6 max-w-5xl mx-auto">
      <div class="inline-flex items-center gap-2 bg-gray-700 backdrop-blur-lg border border-white/20 px-5 py-2.5 rounded-full text-sm mb-8">
        <span class="w-2 h-2 bg-white rounded-full animate-pulse"></span>
        <span class="text-white font-medium">Vektor Interaktif</span>
      </div>
      
      <h1 class="text-5xl md:text-7xl lg:text-8xl font-bold tracking-tight text-gray-700 mb-12">
        <span class="block mb-2">Vector</span>
        <span class="block bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">Visualization</span>
      </h1>
      
      <p class="text-xl md:text-xl text-black/80 mb-10 max-w-3xl mx-auto leading-relaxed font-light">
        Eksplorasi dan pahami konsep vektor secara interaktif melalui
      </p>

      <div class="flex flex-col sm:flex-row gap-5 justify-center items-center">
        <button onclick="document.getElementById('main-tool')?.scrollIntoView({ behavior: 'smooth' })" 
          class="px-10 py-4 bg-gray-800 text-white rounded-2xl font-semibold hover:bg-white/90 transition-all duration-300 flex items-center gap-3 shadow-2xl shadow-white/20 hover:scale-105">
          <span>Mulai Menjelajah</span>
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"></path>
          </svg>
        </button>
      </div>

      <div class="mt-16 flex flex-wrap justify-center gap-8 text-sm text-gray-500">
        <div class="flex items-center gap-2">
          <svg class="w-5 h-5 text-cyan-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
          </svg>
          <span>Visualisasi Interaktif</span>
        </div>
        <div class="flex items-center gap-2">
          <svg class="w-5 h-5 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
          </svg>
          <span>Rumus dasar dan Penulisan</span>
        </div>
        <div class="flex items-center gap-2">
          <svg class="w-5 h-5 text-pink-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 5a1 1 0 011-1h14a1 1 0 011 1v2a1 1 0 01-1 1H5a1 1 0 01-1-1V5zM4 13a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H5a1 1 0 01-1-1v-6zM16 13a1 1 0 011-1h2a1 1 0 011 1v6a1 1 0 01-1 1h-2a1 1 0 01-1-1v-6z"></path>
          </svg>
          <span>3D Visualisasi</span>
        </div>
      </div>
    </div>
  </section>
</template>
