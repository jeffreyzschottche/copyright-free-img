<template>
    <div class="explain">
        <h2>
            Copyright Free Foto's
        </h2>
        <input type="text" v-model="searchTerm" @keyup.enter="searchImages" autofocus/>
        <button @click="searchImages">Zoeken</button>
        <div class="explain-details">
            <b>- Voer je zoekopdracht in en klik op enter of op zoeken</b>
            <b>- Klik op de afbeelding om deze te vergroten </b>
            <b>- Download de afbeelding of ga door naar de volgende door te sluiten</b>
        </div>
      </div>
      <div class="grid">
        <img v-for="image in images" :key="image.id" :src="image.urls.small" alt="image" class="grid-item" @click="openModal(image)">
      </div>
      <modal v-if="showModal" @close="showModal = false">
        <img :src="selectedImage.urls.full" alt="image">
        <div class="actions">
        <button @click="showModal = false">Sluit</button>
        <a href="#" @click="downloadImage">Download</a>
    </div>
    </modal>
  </template>
  
<script>
import axios from 'axios'

export default {
    name: 'SearchComponent',

  data() {
    return {
      searchTerm: '',
      images: [],
      showModal: false,
selectedImage: {}
    }
  },
  created() {
    this.searchImages()
  },
  methods: {
    async searchImages() {
      try {
        const response = await axios.get(
          `https://api.unsplash.com/search/photos?query=${this.searchTerm}&per_page=30`,
          {
            headers: {
              'Authorization': 'Client-ID bvnsrIdi6RmEm3y-VOrGfgqF7qJTLuXFwPZDAL7S7LY'
            }
          }
        )
        this.images = response.data.results
      } catch (error) {
        console.log(error)
      }
    },
    openModal(image) {
this.selectedImage = image
this.showModal = true
},
downloadImage() {
      fetch(this.selectedImage.urls.full)
        .then(res => res.blob())
        .then(blob => {
          const a = document.createElement('a')
          a.href = URL.createObjectURL(blob)
          a.download = 'image.jpg'
          a.click()
        })
    }
  }
}
</script>

  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Architects+Daughter&display=swap');

  *{
    font-family: 'Architects Daughter', cursive;
  }
  .grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .grid-item {
    width: 25%;
    height: 200px;
    padding: 1em;
  }

  modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 9999;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
  
  modal img {
    max-width: 80%;
    max-height: 80%;
  }

  modal button{
    color: white;
    background: black;
    width: 120px;
    height: 50px;
    text-transform: uppercase;
    cursor: pointer;
  }
  modal a{
    color: black;
    font-size: bold;
    text-decoration: none;
    background-color: white;
    padding: 1em;
    width: 120px;
   
  }

  img:hover{
    cursor: pointer;
  }

  .actions{
    display: flex;
    flex-direction: row;
  }
  .explain{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  .explain input{
    width: 200px;
    margin-top: -1em;
    margin-bottom: 0.6em;
  }
  .explain button{
    width: 200px;
    margin-bottom: 0.6em;
    background: black;
    color: white;
    cursor: pointer;
    text-transform: uppercase;
  }
  .explain-details{
    display: flex;
    flex-direction: column;
    border: 2px solid black;
    background: white;
    padding: 1em;
  }
  </style>
  