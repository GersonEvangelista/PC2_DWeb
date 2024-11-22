<template>
  <div class="min-h-screen bg-gray-100 py-8">
    <div class="container mx-auto px-4">
      <h1 class="text-4xl font-bold mb-8 text-center text-gray-800">Buscador de Películas</h1>

      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <label for="search" class="block text-sm font-medium text-gray-700 mb-1">Título</label>
            <input
              id="search"
              v-model="searchQuery"
              @input="debounceSearch"
              placeholder="Buscar por título"
              class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            >
          </div>
          <div>
            <label for="genre" class="block text-sm font-medium text-gray-700 mb-1">Género</label>
            <select
              id="genre"
              v-model="selectedGenre"
              @change="searchMovies"
              class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            >
              <option value="">Todos los géneros</option>
              <option v-for="genre in genres" :key="genre.id" :value="genre.id">{{ genre.name }}</option>
            </select>
          </div>
          <div>
            <label for="year" class="block text-sm font-medium text-gray-700 mb-1">Año</label>
            <input
              id="year"
              v-model="selectedYear"
              @input="debounceSearch"
              type="number"
              placeholder="Año de lanzamiento"
              class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            >
          </div>
        </div>
      </div>

      <div v-if="loading" class="flex justify-center items-center h-64">
        <div class="animate-spin rounded-full h-16 w-16 border-t-2 border-b-2 border-blue-500"></div>
      </div>

      <div v-else-if="movies.length" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div v-for="movie in movies" :key="movie.id" class="bg-white rounded-lg shadow-md overflow-hidden transition-transform duration-300 hover:scale-105">
          <img :src="getPosterUrl(movie.poster_path)" :alt="movie.title" class="w-full h-96 object-cover">
          <div class="p-4">
            <h2 class="text-xl font-semibold mb-2 text-gray-800">{{ movie.title }}</h2>
            <div class="flex items-center justify-between">
              <div class="flex items-center">
                <svg class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                </svg>
                <span class="ml-1 text-gray-600">{{ movie.vote_average.toFixed(1) }}</span>
              </div>
              <span class="text-sm text-gray-500">{{ movie.vote_count }} votos</span>
            </div>
          </div>
        </div>
      </div>

      <div v-else class="text-center text-gray-600 text-xl mt-16">
        <p>No se encontraron películas.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import debounce from 'lodash/debounce'

const API_KEY = 'eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJhMGVkY2NmYjY5ZGFmYTljMzcwYWE3NGY4NmFhMDk5ZSIsIm5iZiI6MTczMjE1OTQzMC4wNTk2NTExLCJzdWIiOiI2NzNlYTQzZmFkZTkzMTBmM2ZkZjk1ZWQiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.uVuMOkpbtZ03jV6r2i4qvkqpaQMoy9AQKNGTI6dQOII'
const API_BASE_URL = 'https://api.themoviedb.org/3'
const POSTER_BASE_URL = 'http://image.tmdb.org/t/p/w500'

const searchQuery = ref('')
const selectedGenre = ref('')
const selectedYear = ref('')
const movies = ref([])
const genres = ref([])
const loading = ref(false)

const searchMovies = async () => {
  loading.value = true
  try {
    const params = {
      api_key: API_KEY,
      language: 'es-ES',
      query: searchQuery.value,
      with_genres: selectedGenre.value,
      year: selectedYear.value,
    }
    const response = await axios.get(`${API_BASE_URL}/search/movie`, { params })
    movies.value = response.data.results
  } catch (error) {
    console.error('Error al buscar películas:', error)
  } finally {
    loading.value = false
  }
}

const debounceSearch = debounce(searchMovies, 300)

const getGenres = async () => {
  try {
    const response = await axios.get(`${API_BASE_URL}/genre/movie/list`, {
      params: { api_key: API_KEY, language: 'es-ES' }
    })
    genres.value = response.data.genres
  } catch (error) {
    console.error('Error al obtener géneros:', error)
  }
}

const getPosterUrl = (posterPath) => {
  return posterPath ? `${POSTER_BASE_URL}${posterPath}` : '/placeholder.jpg'
}

onMounted(() => {
  getGenres()
})
</script>
