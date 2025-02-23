<template>
    <nav class="bg-gradient-to-r from-yellow-300 via-pink-300 to-purple-300 p-4 shadow-lg rounded-b-3xl">
      <div class="container mx-auto flex flex-col md:flex-row items-center justify-between space-y-4 md:space-y-0">
        <!-- Logo -->
        <div class="flex items-center space-x-2">
          <img
            src="https://res.cloudinary.com/dglptwdlb/image/upload/v1734807639/4ed2deb9-d96b-4436-ac66-f1a6e2c9a0e2-profile_image-70x70_lnup9x.png" 
            alt="logo" 
            class="w-12 h-12"
          />
          <span class="text-3xl font-bold text-white drop-shadow-lg">Emmy21of</span>
        </div>
        
        <!-- Links -->
        <div class="hidden md:flex space-x-4 font-semibold">
          <router-link :to="{ name: 'home' }" class="text-blue-700 hover:text-red-500 transition text-xl">Inicio</router-link>
          <router-link :to="{ name: 'galery' }" class="text-green-600 hover:text-orange-400 transition text-xl">Arte</router-link>
          <router-link :to="{ name: 'music' }" class="text-purple-700 hover:text-yellow-400 transition text-xl">Música</router-link>
        </div>
  
        <!-- Botón de Menú Móvil -->
        <div class="md:hidden">
          <button @click="toggleMenu" class="text-white focus:outline-none" aria-label="Toggle Menu">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
            </svg>
          </button>
        </div>
      </div>
  
      <!-- Menú desplegable en móvil -->
      <div v-if="isOpen" class="md:hidden bg-yellow-200 p-4 rounded-lg mt-2 space-y-2">
        <router-link :to="{ name: 'home' }" class="block py-2 text-blue-700 hover:bg-pink-200 rounded-lg">Inicio</router-link>
        <router-link :to="{ name: 'galery' }" class="block py-2 text-green-600 hover:bg-purple-200 rounded-lg">Arte</router-link>
        <router-link :to="{ name: 'music' }" class="block py-2 text-purple-700 hover:bg-yellow-300 rounded-lg">Música</router-link>
      </div>
    </nav>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const isOpen = ref(false);
  
  const toggleMenu = () => {
    isOpen.value = !isOpen.value;
  };
  </script>
  
  <style scoped>
  /* Estilos adicionales */
  </style>
  