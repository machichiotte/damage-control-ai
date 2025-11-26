<script setup>
import { ref } from 'vue'

const API_URL = 'http://127.0.0.1:8000'

const isDragging = ref(false)
const isUploading = ref(false)
const uploadedImage = ref(null)
const uploadResult = ref(null)
const errorMessage = ref(null)

const handleDragOver = (e) => {
  e.preventDefault()
  isDragging.value = true
}

const handleDragLeave = () => {
  isDragging.value = false
}

const handleDrop = (e) => {
  e.preventDefault()
  isDragging.value = false
  
  const files = e.dataTransfer.files
  if (files.length > 0) {
    handleFile(files[0])
  }
}

const handleFileInput = (e) => {
  const files = e.target.files
  if (files.length > 0) {
    handleFile(files[0])
  }
}

const handleFile = async (file) => {
  // Vérifier que c'est une image
  if (!file.type.startsWith('image/')) {
    errorMessage.value = 'Veuillez sélectionner une image'
    return
  }

  // Réinitialiser les états
  errorMessage.value = null
  uploadResult.value = null
  
  // Prévisualisation locale
  const reader = new FileReader()
  reader.onload = (e) => {
    uploadedImage.value = e.target.result
  }
  reader.readAsDataURL(file)

  // Upload vers le backend
  isUploading.value = true
  
  const formData = new FormData()
  formData.append('file', file)

  try {
    const response = await fetch(`${API_URL}/upload`, {
      method: 'POST',
      body: formData
    })

    if (!response.ok) {
      throw new Error('Erreur lors de l\'upload')
    }

    const data = await response.json()
    uploadResult.value = data
  } catch (error) {
    errorMessage.value = error.message
    uploadedImage.value = null
  } finally {
    isUploading.value = false
  }
}

const reset = () => {
  uploadedImage.value = null
  uploadResult.value = null
  errorMessage.value = null
}
</script>

<template>
  <div class="w-full max-w-2xl">
    <!-- Zone d'upload -->
    <div
      v-if="!uploadedImage"
      @dragover="handleDragOver"
      @dragleave="handleDragLeave"
      @drop="handleDrop"
      :class="[
        'border-2 border-dashed rounded-xl p-12 text-center transition-all duration-300 cursor-pointer',
        isDragging 
          ? 'border-blue-500 bg-blue-500/10 scale-105' 
          : 'border-slate-600 hover:border-blue-400 hover:bg-slate-800/50'
      ]"
    >
      <input
        type="file"
        accept="image/*"
        @change="handleFileInput"
        class="hidden"
        id="file-input"
      />
      <label for="file-input" class="cursor-pointer">
        <div class="text-6xl mb-4">📸</div>
        <p class="text-xl font-semibold mb-2">
          {{ isDragging ? 'Déposez l\'image ici' : 'Glissez une photo ou cliquez' }}
        </p>
        <p class="text-sm text-gray-400">
          Formats acceptés : JPG, PNG, WEBP
        </p>
      </label>
    </div>

    <!-- Prévisualisation et résultat -->
    <div v-else class="space-y-6">
      <!-- Image uploadée -->
      <div class="relative rounded-xl overflow-hidden bg-slate-800 border border-slate-700">
        <img 
          :src="uploadedImage" 
          alt="Image uploadée" 
          class="w-full h-auto"
        />
        <button
          @click="reset"
          class="absolute top-4 right-4 bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-colors"
        >
          ✕ Supprimer
        </button>
      </div>

      <!-- Loader -->
      <div v-if="isUploading" class="text-center py-8">
        <div class="inline-block animate-spin rounded-full h-12 w-12 border-4 border-blue-500 border-t-transparent"></div>
        <p class="mt-4 text-gray-400">Analyse en cours...</p>
      </div>

      <!-- Résultat -->
      <div v-else-if="uploadResult" class="bg-slate-800 rounded-xl p-6 border border-slate-700">
        <h3 class="text-xl font-semibold mb-4 text-green-400">✓ Upload réussi</h3>
        <div class="space-y-2 text-sm">
          <div class="flex justify-between">
            <span class="text-gray-400">Nom du fichier :</span>
            <span class="font-mono">{{ uploadResult.filename }}</span>
          </div>
          <div class="flex justify-between">
            <span class="text-gray-400">Taille :</span>
            <span>{{ (uploadResult.size / 1024).toFixed(2) }} KB</span>
          </div>
          <div class="flex justify-between">
            <span class="text-gray-400">Date :</span>
            <span>{{ new Date(uploadResult.uploaded_at).toLocaleString('fr-FR') }}</span>
          </div>
        </div>
      </div>

      <!-- Erreur -->
      <div v-else-if="errorMessage" class="bg-red-500/10 border border-red-500 rounded-xl p-6">
        <p class="text-red-400">❌ {{ errorMessage }}</p>
      </div>
    </div>
  </div>
</template>
