<script setup>
import axios from "axios"
const props = defineProps(["id"]);
const emits = defineEmits(["toggleModal"]);

let movieinfo = (
  await axios.get(`https://api.themoviedb.org/3/movie/${props.id}`, {
    params: {
      api_key: "64688a2ee40628130c4073aff0308684",
      append_to_response: "videos",
    },
  })
).data;
console.log(movieinfo);
</script>

<template>
  <Teleport to="body">
    <div class="modal-outer-container" @click.self="emits('toggleModal')">
      <div class="modal-inner-container">
        <button class="close-button" @click="emits('toggleModal')">X</button>
        <h1>{{ movieinfo.original_title}}</h1>
        <h1>{{ movieinfo.release_date}}</h1>
        <img :src="`https://image.tmdb.org/t/p/w500${movieinfo.poster_path}`" alt="">
        <iframe :src="`https://www.youtube.com/embed/${movieinfo.videos.results.filter((video) => video.type === 'Trailer').at(0).key}`"></iframe>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
h1{
  text-align: center;
}

iframe{
  position: absolute;
  aspect-ratio: 3/2;
  width: 450px;
  align-self: center;
  margin: 20px;
  margin-left: 80px;
}

img{
  aspect-ratio: 2/3;
  width: 200px;
  margin: 20px;
}

.modal-outer-container {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background: #00000099;
  z-index: 3;
}

.modal-outer-container .modal-inner-container {
  background-color: #1f2123;
  color: white;
  width: clamp(280px, 100%, 800px);
  height: 400px;
  position: relative;
}

.modal-outer-container .modal-inner-container .close-button {
  position: absolute;
  right: 0px;
  padding: 1rem;
  border: none;
  background: #1f2123;
  font-weight: bold;
  font-size: 1.25rem;
  color: white;
}
</style>
