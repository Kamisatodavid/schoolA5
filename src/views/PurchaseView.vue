<script setup>
import { ref } from 'vue';
import SiteModal from '../components/SiteModal.vue';
import axios from 'axios'

const showModal = ref(false);
const selectedId = ref(0);
const openModal = (id) => {
  showModal.value = true;
  selectedId.value = id;
};
const closeModal = () => {
  showModal.value = false;
};


let data = (
  await axios.get(`https://api.themoviedb.org/3/trending/movie/week`, {
    params: {
      api_key: "64688a2ee40628130c4073aff0308684",
    },
  })
).data.results;
console.log(data);
</script>

<template>
  <div class="moviePoster-container">
    <img v-for="movie in data" @click="openModal(movie.id)" class="poster" :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
  </div>
   <SiteModal v-if="showModal" @toggleModal="closeModal()" :id="selectedId" />
</template>

<style scoped>
img{
  margin: 10px;
  aspect-ratio: 2/3;
  width: 230px;
}

.moviePoster-container{
  background-color: black;
}

</style>