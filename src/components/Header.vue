<template>
  <nav class="backdrop-blur-md fixed w-full z-50">
    <div class="max-w-7xl mx-auto px-6 py-5 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <div class="w-6 h-6 bg-black rounded flex items-center justify-center text-white font-bold text-xl">•</div>
        <span class="text-2xl font-semibold tracking-tighter">Vector</span>
      </div>
      <div class="hidden md:flex items-center gap-8">
        <a href="#hero" class="text-gray-700 text-md hover:text-white transition">Beranda</a>
        <a href="#tool" class="text-gray-700 text-md hover:text-white transition">Sejarah</a>
        <a href="#tool" class="text-gray-700 text-md hover:text-white transition">Visi Misi</a>
        <a href="#tool" class="text-gray-700 text-md hover:text-white transition">Struktur</a>
        <a href="#about" class="text-gray-700 text-md hover:text-white transition">Tentang</a>
      </div>
      
      <!-- Mobile menu button -->
      <button class="md:hidden text-gray-700 text-md hover:text-white">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
        </svg>
      </button>
    </div>
  </nav>
</template>