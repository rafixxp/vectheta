<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const observer = ref(null)
const memberSection = ref(null)

const setupIntersectionObserver = () => {
  observer.value = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const cards = entry.target.querySelectorAll('.member-card')
          cards.forEach((card, index) => {
            setTimeout(() => {
              card.classList.add('animate-fade-in')
            }, index * 100)
          })
          observer.value.unobserve(entry.target)
        }
      })
    },
    {
      threshold: 0.1,
      rootMargin: '0px 0px -50px 0px'
    }
  )
}

onMounted(() => {
  setupIntersectionObserver()
  if (memberSection.value) {
    observer.value.observe(memberSection.value)
  }
})

onUnmounted(() => {
  if (observer.value) {
    observer.value.disconnect()
  }
})
</script>

<template>
    <section id="member" ref="memberSection" class="py-24 bg-white">
        <div class="max-w-6xl mx-auto px-6">
            <div class="text-center mb-16">
                <h1 class="text-3xl md:text-4xl font-bold text-gray-900 mb-4">Anggota Tim</h1>
                <p class="text-gray-500 text-lg">Tim pengembang di balik proyek aplikasi ini</p>
            </div>
            
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
                <div class="member-card bg-white rounded-2xl p-8 shadow-xs border border-gray-100 flex flex-col items-center text-center hover:shadow-md transition-all duration-300 ease-in opacity-0">
                    <img src="/akmal.webp" alt="Akmal Rijaludin" class="w-32 h-32 rounded-full object-cover mb-6">
                    <h4 class="text-lg font-semibold text-gray-900 mb-2">Akmal Rijaludin</h4>
                    <p class="text-sm text-gray-500 font-mono">257006111043</p>
                </div>
                
                <div class="member-card bg-white rounded-2xl p-8 shadow-xs border border-gray-100 flex flex-col items-center text-center hover:shadow-md transition-all duration-300 ease-in opacity-0">
                    <img src="/fauzi.webp" alt="Fauzi Awaludin Kamal" class="w-32 h-32 rounded-full object-cover mb-6">
                    <h4 class="text-lg font-semibold text-gray-900 mb-2">Fauzi Awaludin Kamal</h4>
                    <p class="text-sm text-gray-500 font-mono">257006111027</p>
                </div>
                
                <div class="member-card bg-white rounded-2xl p-8 shadow-xs border border-gray-100 flex flex-col items-center text-center hover:shadow-md transition-all duration-300 ease-in opacity-0">
                    <img src="/nasikha.webp" alt="Muhammad Nasikha Rahman" class="w-32 h-32 rounded-full object-cover mb-6">
                    <h4 class="text-lg font-semibold text-gray-900 mb-2">Muhammad Nasikha Rahman</h4>
                    <p class="text-sm text-gray-500 font-mono">257006111058</p>
                </div>
                
                <div class="member-card bg-white rounded-2xl p-8 shadow-xs border border-gray-100 flex flex-col items-center text-center hover:shadow-md transition-all duration-300 ease-in opacity-0">
                    <img src="/sami.webp" alt="Muhammad Fakhir Sami Wiraatmaja" class="w-32 h-32 rounded-full object-cover mb-6">
                    <h4 class="text-lg font-semibold text-gray-900 mb-2">Muhammad Fakhir Sami</h4>
                    <p class="text-sm text-gray-500 font-mono">257006111057</p>
                </div>
                
                <div class="member-card bg-white rounded-2xl p-8 shadow-xs border border-gray-100 flex flex-col items-center text-center hover:shadow-md transition-all duration-300 ease-in opacity-0">
                    <img src="/profile.webp" alt="Rafi Ahfa Fauzan" class="w-32 h-32 rounded-full object-cover mb-6">
                    <h4 class="text-lg font-semibold text-gray-900 mb-2">Rafi Ahfa Fauzan</h4>
                    <p class="text-sm text-gray-500 font-mono">257006111017</p>
                </div>
                
                <div class="member-card bg-white rounded-2xl p-8 shadow-xs border border-gray-100 flex flex-col items-center text-center hover:shadow-md transition-all duration-300 ease-in opacity-0">
                    <img src="/syayid.webp" alt="Syayid Nurul Fadilah" class="w-32 h-32 rounded-full object-cover mb-6">
                    <h4 class="text-lg font-semibold text-gray-900 mb-2">Syayid Nur Fadilah</h4>
                    <p class="text-sm text-gray-500 font-mono">257006111062</p>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
@keyframes fade-in {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.animate-fade-in {
    animation: fade-in 0.6s ease-out forwards;
}
</style>
