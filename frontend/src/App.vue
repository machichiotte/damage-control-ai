<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import ImageUploader from './components/ImageUploader.vue'
import ContractUploader from './components/ContractUploader.vue'
import ClaimEvaluator from './components/ClaimEvaluator.vue'
import { API_URL } from './config.js'

const activeTab = ref('image')
const backendStatus = ref('checking') // 'checking', 'connected', 'disconnected'
let healthCheckInterval = null

// Vérifier la santé du backend
const checkBackendHealth = async () => {
  try {
    const response = await fetch(`${API_URL}/health`, {
      method: 'GET',
      signal: AbortSignal.timeout(5000) // Timeout de 5 secondes
    })

    if (response.ok) {
      backendStatus.value = 'connected'
    } else {
      backendStatus.value = 'disconnected'
    }
  } catch (error) {
    console.error('Backend health check failed:', error)
    backendStatus.value = 'disconnected'
  }
}

// Au montage du composant
onMounted(() => {
  // Vérifier immédiatement
  checkBackendHealth()

  // Puis vérifier toutes les 30 secondes
  healthCheckInterval = setInterval(checkBackendHealth, 30000)
})

// Nettoyer l'interval au démontage
onUnmounted(() => {
  if (healthCheckInterval) {
    clearInterval(healthCheckInterval)
  }
})
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4 sm:p-6 md:p-8">
    <div class="text-center mb-12">
      <h1
        class="text-4xl sm:text-4xl md:text-5xl lg:text-6xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 via-purple-400 to-emerald-400 mb-4 animate-gradient"
        style="padding-bottom: 1rem;">
        DamageControl AI
      </h1>
      <p class="text-lg sm:text-xl text-gray-400 px-4">L'Expert en Sinistres Automatisé</p>
      <div class="flex gap-4 justify-center mt-4 text-sm">
        <span v-if="backendStatus === 'checking'" class="flex items-center gap-2 text-gray-500">
          <span class="w-2 h-2 bg-gray-500 rounded-full animate-pulse"></span>
          Vérification du backend...
        </span>
        <span v-else-if="backendStatus === 'connected'" class="flex items-center gap-2 text-green-400">
          <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
          Backend connecté
        </span>
        <span v-else class="flex items-center gap-2 text-red-400">
          <span class="w-2 h-2 bg-red-500 rounded-full"></span>
          Backend déconnecté
        </span>
      </div>
    </div>

    <!-- Tabs -->
    <div class="flex flex-col sm:flex-row gap-3 sm:gap-4 mb-8 w-full max-w-2xl px-4">
      <button @click="activeTab = 'image'" :class="[
        'px-4 sm:px-6 py-3 rounded-xl font-semibold transition-all text-sm sm:text-base',
        activeTab === 'image'
          ? 'bg-gradient-to-r from-blue-500 to-purple-500 text-white shadow-lg'
          : 'bg-slate-800/50 text-gray-400 hover:bg-slate-700/50'
      ]">
        📸 Analyser une photo
      </button>
      <button @click="activeTab = 'contract'" :class="[
        'px-4 sm:px-6 py-3 rounded-xl font-semibold transition-all text-sm sm:text-base',
        activeTab === 'contract'
          ? 'bg-gradient-to-r from-purple-500 to-emerald-500 text-white shadow-lg'
          : 'bg-slate-800/50 text-gray-400 hover:bg-slate-700/50'
      ]">
        📄 Analyser un contrat
      </button>
      <button @click="activeTab = 'evaluate'" :class="[
        'px-4 sm:px-6 py-3 rounded-xl font-semibold transition-all text-sm sm:text-base',
        activeTab === 'evaluate'
          ? 'bg-gradient-to-r from-emerald-500 to-teal-500 text-white shadow-lg'
          : 'bg-slate-800/50 text-gray-400 hover:bg-slate-700/50'
      ]">
        🔍 Évaluer le sinistre
      </button>
    </div>

    <!-- Main Content -->
    <ImageUploader v-if="activeTab === 'image'" />
    <ContractUploader v-if="activeTab === 'contract'" />
    <ClaimEvaluator v-if="activeTab === 'evaluate'" />

    <!-- Info Cards -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full max-w-4xl mt-12">
      <div
        class="bg-slate-800/50 backdrop-blur-sm p-6 rounded-xl border border-slate-700/50 hover:border-blue-500/50 transition-all">
        <div class="text-3xl mb-3">🎯</div>
        <h3 class="font-semibold mb-2">Depth Estimation</h3>
        <p class="text-sm text-gray-400">Analyse 3D de la gravité des impacts</p>
      </div>

      <div
        class="bg-slate-800/50 backdrop-blur-sm p-6 rounded-xl border border-slate-700/50 hover:border-purple-500/50 transition-all">
        <div class="text-3xl mb-3">🔍</div>
        <h3 class="font-semibold mb-2">Détection IA</h3>
        <p class="text-sm text-gray-400">Identification des pièces endommagées</p>
      </div>

      <div
        class="bg-slate-800/50 backdrop-blur-sm p-6 rounded-xl border border-slate-700/50 hover:border-emerald-500/50 transition-all">
        <div class="text-3xl mb-3">📄</div>
        <h3 class="font-semibold mb-2">Analyse Contrat</h3>
        <p class="text-sm text-gray-400">Extraction des garanties et franchises</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes gradient {

  0%,
  100% {
    background-position: 0% 50%;
  }

  50% {
    background-position: 100% 50%;
  }
}

.animate-gradient {
  background-size: 200% 200%;
  animation: gradient 3s ease infinite;
}
</style>
