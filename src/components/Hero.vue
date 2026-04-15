<template>
  <section class="min-h-screen relative flex items-center hero">
    <!-- Three.js Background -->
    <div id="hero-canvas" class="absolute inset-0 z-0 opacity-40"></div>

    <div class="max-w-5xl mx-auto px-6 relative z-10 pt-2">
      <div class="text-center max-w-3xl mx-auto">
        <div class="inline-flex items-center gap-2 bg-dark-900 border border-zinc-700 px-5 py-2 rounded-full text-sm mb-6">
          <span class="text-emerald-400">●</span>
          Pemodelan Dot Product Vektor 3D
        </div>
        
        <h1 class="text-6xl md:text-7xl font-semibold tracking-tighter leading-none mb-6">
          Kalkulasi.<br>
          Visualisasikan.<br>
          <span class="bg-gradient-to-r from-zinc-800 to-gray-500 bg-clip-text text-transparent">dan Mengerti.</span>
        </h1>
        
        <p class="text-xl text-zinc-400 mb-10 max-w-lg mx-auto">
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
