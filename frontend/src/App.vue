<script setup>
import ImageUploader from './components/ImageUploader.vue'
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-8">
    <!-- Header -->
    <div class="text-center mb-12">
      <h1 class="text-6xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 via-purple-400 to-emerald-400 mb-4 animate-gradient">
        DamageControl AI
      </h1>
      <p class="text-xl text-gray-400">L'Expert en Sinistres Automatisé</p>
      <div class="flex gap-4 justify-center mt-4 text-sm text-gray-500">
        <span class="flex items-center gap-2">
          <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
          Backend connecté
        </span>
      </div>
    </div>
    
    <!-- Main Content -->
    <ImageUploader />

    <!-- Info Cards -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full max-w-4xl mt-12">
      <div class="bg-slate-800/50 backdrop-blur-sm p-6 rounded-xl border border-slate-700/50 hover:border-blue-500/50 transition-all">
        <div class="text-3xl mb-3">🎯</div>
        <h3 class="font-semibold mb-2">Depth Estimation</h3>
        <p class="text-sm text-gray-400">Analyse 3D de la gravité des impacts</p>
      </div>
      
      <div class="bg-slate-800/50 backdrop-blur-sm p-6 rounded-xl border border-slate-700/50 hover:border-purple-500/50 transition-all">
        <div class="text-3xl mb-3">🔍</div>
        <h3 class="font-semibold mb-2">Détection IA</h3>
        <p class="text-sm text-gray-400">Identification des pièces endommagées</p>
      </div>
      
      <div class="bg-slate-800/50 backdrop-blur-sm p-6 rounded-xl border border-slate-700/50 hover:border-emerald-500/50 transition-all">
        <div class="text-3xl mb-3">📄</div>
        <h3 class="font-semibold mb-2">Analyse Contrat</h3>
        <p class="text-sm text-gray-400">Extraction des garanties et franchises</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes gradient {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.animate-gradient {
  background-size: 200% 200%;
  animation: gradient 3s ease infinite;
}
</style>
