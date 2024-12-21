<script setup>
import Navbar from '@/components/Navbar.vue';
import { ref, onMounted } from 'vue';

// Variable reactiva para almacenar los datos
const data = ref([]);
const loading = ref(true);
const error = ref(null);


// Función para realizar la solicitud GET
const fetchData = async () => {
  try {
    loading.value = true; // Indica que está cargando
    const response = await fetch('https://res.cloudinary.com/dglptwdlb/raw/upload/v1733686551/lista_dbccve.json'); // Cambia la URL por tu endpoint
    if (!response.ok) {
      throw new Error(`Error: ${response.status}`); // Maneja errores HTTP
    }
    data.value = await response.json(); // Convierte la respuesta a JSON
    console.log(data.value)
  } catch (err) {
    error.value = err.message; // Guarda el mensaje de error
    console.error('Error fetching data:', err);
  } finally {
    loading.value = false; // Detén el estado de carga
  }
};

// Llama a la función cuando el componente se monta
onMounted(()=>{
  fetchData()
});

</script>

<template>
  <div class="bg-custom-image min-h-screen">
    <Navbar />
    <h1 class="text-3xl font-bold underline text-center text-white pt-10">
      Hello world!
    </h1>
    <!-- Agrega más contenido aquí -->
  </div>
</template>

<style scoped>
/* Definimos el fondo de pantalla */
.bg-custom-image {
  background-image: url('https://res.cloudinary.com/dglptwdlb/image/upload/v1731273386/Emmy_Campfire_afjyvf.png'); /* Cambia esta URL por la de tu imagen */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
</style>
