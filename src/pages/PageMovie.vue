<template>
  <div class="movie-page">
    <div class="movie-filter q-ml-md q-mr-xl">
      <MovieFilter @yearCambiada="actualizarYear" />
    </div>
    <div class="movie-list">
      <MovieList :yearFiltrada="yearSeleccionada" />
    </div>
  </div>
</template>

<style>
.movie-page {
  display: flex;
  justify-content: space-between;
}

.movie-filter {
  width: 25%;
}

.movie-list {
  width: 75%;
}
</style>

<script>
import MovieFilter from "src/components/Movie/MovieFilter.vue";
import MovieList from "src/components/Movie/MovieList.vue";

export default {
  name: "MoviePage",
  components: { MovieFilter, MovieList },
  data() {
    return {
      yearSeleccionada: null,
    };
  },
  methods: {
    actualizarCategoria(year) {
      console.log("El year desde MovieFilter es: " + year);
      this.yearSeleccionada = year;
    },
  },
};
</script>
