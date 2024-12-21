<template>
  <div class="p-4">
    <div v-if="loading" class="flex justify-center items-center min-h-screen">
      <div class="animate-spin rounded-full h-16 w-16 border-t-4 border-blue-500"></div>
    </div>

    <div v-else-if="error" class="text-center text-red-600 font-bold">
      <p>Error al cargar las imágenes: {{ error }}</p>
    </div>

    <!-- Grid de imágenes -->
    <div v-else class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
      <div
        v-for="(image, index) in images"
        :key="index"
        class="relative group cursor-pointer"
      >
        <!-- Imagen ajustada al contenedor -->
        <img
          :src="image.url"
          :alt="image.description || 'Imagen de galería'"
          class="w-full h-auto object-cover rounded shadow"
         
        />
        <!-- Información superpuesta al pasar el cursor -->
        <div
          class="absolute inset-0 flex flex-col items-center justify-center bg-black bg-opacity-60 text-white text-center opacity-0 group-hover:opacity-100 transition-opacity"
          @click="openImage(index)" 
          >
          <p class="p-2 text-sm sm:text-base lg:text-lg">
            Autor: {{ image.autor || "" }}
          </p>
          <p class="p-2 text-sm sm:text-base lg:text-lg">
            {{ image.description || "" }}
          </p>
          <a
            :href="image.red_social || ''"
            target="_blank"
            rel="noopener noreferrer"
            class="text-green-200 hover:text-green-400 hover:underline cursor-pointer transition"
          >
            Visitar Red Social
          </a>
        </div>
      </div>
    </div>

    <!-- Visor de imágenes -->
    <ImageViewer
      v-if="currentIndex !== null"
      :images="images"
      :current-index="currentIndex"
      @close="closeViewer"
      @prev="prevImage"
      @next="nextImage"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import ImageViewer from "./ImageComponent.vue";

// Estado reactivo para imágenes, loading y error
const images = ref([]);
const loading = ref(true);
const error = ref(null);

// Función para cargar las imágenes desde una API
const fetchData = async () => {
  try {
    loading.value = true;
    const response = await fetch(
      "https://res.cloudinary.com/dglptwdlb/raw/upload/v1733686551/lista_dbccve.json"
    );
    if (!response.ok) throw new Error(`Error: ${response.status}`);
    images.value = await response.json();
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};

// Cargar datos al montar
onMounted(() => fetchData());

// Estado del visor
const currentIndex = ref(null);
const openImage = (index) => (currentIndex.value = index);
const closeViewer = () => (currentIndex.value = null);

// Navegación circular
const prevImage = () => {
  currentIndex.value =
    currentIndex.value === 0 ? images.value.length - 1 : currentIndex.value - 1;
};
const nextImage = () => {
  currentIndex.value =
    currentIndex.value === images.value.length - 1 ? 0 : currentIndex.value + 1;
};
</script>

<style>
/* Desenfoque para el fondo del visor */
.backdrop-blur-md {
  backdrop-filter: blur(10px);
}
</style>
