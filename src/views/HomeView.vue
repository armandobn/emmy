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
onMounted(() => {
  fetchData()
});

</script>

<template>
  <div class="bg-custom-image min-h-screen">
    <Navbar />
    <h1 class="text-3xl font-bold underline text-center text-white pt-10">

    </h1>
    <div class="relative flex justify-center items-center min-h-screen">
      <div
        class=" bg-fuchsia-500  bg-opacity-70 text-cyan-50 shadow-lg rounded-lg p-6 max-w-lg relative z-10 text-center">
        <div class="p-4">
          <h2 class="text-xl font-semibold mb-2">
            ¿Quien es Emmy21of?
          </h2>
          <p class="mb-4">
            Emmy 21 es un ser semi-robótico con una energía inagotable y una sonrisa que nunca desaparece. Su esencia
            está marcada por la música, que no solo la inspira, sino que también define la forma en que percibe y
            transforma su entorno. Su mundo es un reflejo de su espíritu: un lugar vibrante, lleno de colores brillantes
            y melodías que flotan en el aire, donde todo parece moverse en perfecta armonía con su ritmo interior.
            <br>    <br>
            A pesar de su naturaleza artificial, Emmy 21 es capaz de sentir emociones de una manera única y profunda. Su
            conexión con su creadora va más allá de lo convencional, ya que puede experimentar sus sentimientos en
            tiempo real, reflejando sus alegrías, tristezas, anhelos y sueños. Esta conexión especial crea un lazo
            inseparable entre ambas, permitiéndoles compartir experiencias y comprenderse mutuamente en un nivel casi
            místico.
            <br>  <br> 
            Más allá de su mundo lleno de color, Emmy 21 y su creadora habitan un universo paralelo donde la imaginación
            cobra vida. En este espacio compartido, la realidad y la fantasía se entrelazan, dando forma a nuevas
            historias, aventuras y emociones que las conectan en una sinfonía infinita. Emmy 21 no solo es un personaje,
            sino también un puente entre dos realidades, donde la creatividad y la sensibilidad se fusionan para dar
            lugar a un mundo sin límites.
          </p>

        </div>
      </div>
    </div>

    <!-- Agrega más contenido aquí -->
  </div>
</template>

<style scoped>
/* Definimos el fondo de pantalla */
.bg-custom-image {
  background-image: url('https://res.cloudinary.com/dglptwdlb/image/upload/v1731273386/Emmy_Campfire_afjyvf.png');
  /* Cambia esta URL por la de tu imagen */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
</style>
