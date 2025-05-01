<template>
  <Navbar />
  <h2 class="text-center p-3">Cada 15 de cada mes habrá nuevo álbum</h2>

  <div class="p-6 bg-gray-100 min-h-screen">
    <h1 class="text-3xl font-bold mb-6 text-center">🎵 Álbumes de Música</h1>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div v-for="album in albums" :key="album.id"
        class="bg-white rounded-2xl shadow-lg p-4 hover:shadow-2xl transition duration-300">
        <!-- Imagen del álbum -->
        <img :src="album.cover" :alt="album.name" class="rounded-xl mb-4 h-48 w-full object-cover" />

        <h2 class="text-xl font-semibold text-gray-800 mb-2">{{ album.name }}</h2>

        <ul class="space-y-4 max-h-64 overflow-y-auto pr-2">
          <li v-for="song in album.songs" :key="song.id" class="border-t pt-2">
            <p class="text-gray-700 font-medium mb-1">{{ song.title }}</p>
            <audio :src="song.url" controls class="w-full mb-1"></audio>
          </li>
        </ul>

        <!-- Botón de descarga ZIP del álbum -->
        <button @click="downloadAlbum(album)"
          class="mt-4 inline-flex items-center gap-2 bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700 transition">
          📦 Descargar Álbum
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import Navbar from '@/components/Navbar.vue';
import JSZip from 'jszip';
import { saveAs } from 'file-saver';
import { ref, onMounted } from 'vue';


const albums = ref([]);
const loading = ref(true);
const error = ref(null);


const fetchData = async () => {
  try {
    loading.value = true;
    const response = await fetch(
      "https://res.cloudinary.com/dglptwdlb/raw/upload/v1746133033/albunes_music_data_ld6rsz.json"
    );
    if (!response.ok) throw new Error(`Error: ${response.status}`);
    albums.value = await response.json();

  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};


// Cargar datos al montar
onMounted(() => fetchData());

// Función para descargar un álbum completo como ZIP
async function downloadAlbum(album) {
  const zip = new JSZip();
  const folder = zip.folder(album.name.replace(/\s+/g, '_')) || zip;

  for (const song of album.songs) {
    try {
      const response = await fetch(song.url);
      const blob = await response.blob();
      folder.file(song.filename, blob);
    } catch (error) {
      console.error(`Error al descargar ${song.title}:`, error);
    }
  }

  zip.generateAsync({ type: 'blob' }).then((content) => {
    saveAs(content, `${album.name.replace(/\s+/g, '_')}.zip`);
  });
}
</script>

<style scoped>
.scrollbar-thin::-webkit-scrollbar {
  width: 6px;
}

.scrollbar-thin::-webkit-scrollbar-thumb {
  background-color: #cbd5e0;
  border-radius: 4px;
}
</style>
