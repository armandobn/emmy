<template>
    <div
      class="fixed inset-0 flex items-center justify-center z-50 bg-cover bg-center bg-no-repeat"
      :style="{ backgroundImage: `url(${images[currentIndex].url})` }"
      @keydown.esc="close"
      tabindex="0"
    >
      <div class="absolute inset-0 bg-black bg-opacity-70 backdrop-blur-md"></div>
  
      <button
        @click="close"
        class="absolute top-4 right-4 text-white text-2xl z-50"
      >
        &times;
      </button>
  
      <div class="relative z-50">
        <img
          :src="images[currentIndex].url"
          :alt="images[currentIndex].description || 'Imagen seleccionada'"
          class="max-h-[80vh] max-w-[90vw] rounded shadow-lg"
        />
  
        <button
          @click="prev"
          class="absolute top-1/2 left-4 transform -translate-y-1/2 bg-white text-black rounded-full w-10 h-10 flex items-center justify-center shadow"
        >
          &larr;
        </button>
        <button
          @click="next"
          class="absolute top-1/2 right-4 transform -translate-y-1/2 bg-white text-black rounded-full w-10 h-10 flex items-center justify-center shadow"
        >
          &rarr;
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { onMounted, onUnmounted,defineProps, defineEmits  } from "vue";

  
  // Props y eventos
  defineProps({
    images: Array,
    currentIndex: Number,
  });
  const emit = defineEmits(["close", "prev", "next"]);
  
  // Cerrar con tecla "Escape"
  const close = () => emit("close");
  const prev = () => emit("prev");
  const next = () => emit("next");
  
  // Manejo de eventos del teclado
  const handleKeydown = (event) => {
    if (event.key === "ArrowLeft") prev();
    if (event.key === "ArrowRight") next();
  };
  
  onMounted(() => window.addEventListener("keydown", handleKeydown));
  onUnmounted(() => window.removeEventListener("keydown", handleKeydown));
  </script>
  
  <style>
  .backdrop-blur-md {
    backdrop-filter: blur(10px);
  }
  </style>
  