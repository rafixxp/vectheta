<script setup>
const scrollTo = (page) => {
  const element = document.getElementById(page);
  if(element){
    element.scrollIntoView({ behavior: 'smooth' });
  }
}
</script>

<template>
  <nav class="backdrop-blur-md fixed w-full z-50">
    <div class="max-w-7xl mx-auto px-6 py-5 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <div class="w-6 h-6 bg-black rounded flex items-center justify-center text-gray-400 font-bold text-xl">•</div>
        <span class="text-2xl font-semibold tracking-tighter">Vectheta</span>
      </div>
      <div class="hidden md:flex items-center gap-5">
        <div class="group flex items-center">
          <button @click="scrollTo('hero')" class="flex items-center p-2 rounded-lg hover:bg-gray-100 transition-colors">
            <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"></path>
            </svg>
            <span class="ml-0 mr-0 text-gray-700 opacity-0 max-w-0 overflow-hidden transition-all duration-300 group-hover:opacity-100 group-hover:max-w-24 group-hover:ml-2">
              Beranda
            </span>
          </button>
        </div>

        <div class="group flex items-center">
          <button @click="scrollTo('member')" class="flex items-center p-2 rounded-lg hover:bg-gray-100 transition-colors">
            <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z"></path>
            </svg>
            <span class="ml-0 mr-0 text-gray-700 opacity-0 max-w-0 overflow-hidden transition-all duration-300 group-hover:opacity-100 group-hover:max-w-24 group-hover:ml-2">
              Anggota
            </span>
          </button>
        </div>

        <div class="group flex items-center">
          <button @click="scrollTo('history')" class="flex items-center p-2 rounded-lg hover:bg-gray-100 transition-colors">
            <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
            <span class="ml-0 mr-0 text-gray-700 opacity-0 max-w-0 overflow-hidden transition-all duration-300 group-hover:opacity-100 group-hover:max-w-24 group-hover:ml-2">
              Sejarah
            </span>
          </button>
        </div>

        <div class="group flex items-center">
          <button @click="scrollTo('main-tool')" class="flex items-center p-2 rounded-lg hover:bg-gray-100 transition-colors">
            <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12l3-3 3 3 4-4M8 21l4-4 4 4M3 4h18M4 4h16v12a1 1 0 01-1 1H5a1 1 0 01-1-1V4z"></path>
            </svg>
            <span class="ml-0 mr-0 text-gray-700 opacity-0 max-w-0 overflow-hidden transition-all duration-300 group-hover:opacity-100 group-hover:max-w-32 group-hover:ml-2">
              Visualisasi
            </span>
          </button>
        </div>

        <div class="group flex items-center">
          <button @click="scrollTo('about')" class="flex items-center p-2 rounded-lg hover:bg-gray-100 transition-colors">
            <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
            <span class="ml-0 mr-0 text-gray-700 opacity-0 max-w-0 overflow-hidden transition-all duration-300 group-hover:opacity-100 group-hover:max-w-24 group-hover:ml-2">
              Tentang
            </span>
          </button>
        </div>
      </div>
      
      <!-- Mobile menu button -->
      <button class="md:hidden text-gray-700 text-md hover:text-gray-400">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
        </svg>
      </button>
    </div>
  </nav>
</template>