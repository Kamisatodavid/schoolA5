<script setup>
import { ref } from "vue";
import SiteModal from "../components/SiteModal.vue";
import axios from "axios";
import { useStore } from "../store/index.js"

const store = useStore();
const genre = ref(28);
const showModal = ref(false);
const selectedId = ref(0);

const openModal = (id) => {
  showModal.value = true;
  selectedId.value = id;
};

const closeModal = () => {
  showModal.value = false;
};

const getGenres = async () => {
  await store.getMovies(genre.value);
}
let data = (
  await axios.get(`https://api.themoviedb.org/3/trending/movie/week`, {
    params: {
      api_key: "64688a2ee40628130c4073aff0308684",
    },
  })
).data.results;
</script>

<template>
  <div class="moviePoster-container">
    <select v-model="genre" @change="getGenres()">
      <option value="28">Action</option>
      <option value="12">Family</option>
      <option value="16">Science Fiction</option>
      <option value="35">Adventure</option>
      <option value="80">Fantasy</option>
    </select>
    <RouterLink to="/cart" custom v-slot="{ navigate }">
      <button @click="navigate" role="link">Cart</button>
    </RouterLink>
     <img v-for="movie in store.movies" :id="movie.id" @click="openModal(movie.id)"
      :src="`https://image.tmdb.org/t/p/w500${movie.poster}`" />
  </div>
  <SiteModal v-if="showModal" @toggleModal="closeModal()" :id="selectedId" />
</template>

<style scoped>
img {
  margin: 10px;
  aspect-ratio: 2/3;
  width: 230px;
}

.moviePoster-container {
  background-color: black;
}
</style>