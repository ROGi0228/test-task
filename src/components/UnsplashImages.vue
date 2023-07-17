<template>
  <div>
    <h1>Unsplash Images</h1>

    <div class="search__block">
       <span>Поиск по ключевому слову:</span>
        <p style="white-space: pre-line;">{{ message }}</p>
        <input v-model="searchKeyword" placeholder="введите ключевое слово" class="search">
   </div>
    <div v-if="loading">Loading...</div>
    <div v-else class="image_class-wrapper">

       <div v-for="image in filteredImages" :key="image.id" class="image_card">
         <p>{{ image.alt_description }}</p>
         <img :src="image.urls.full" :alt="image.alt_description" width="200" height="200" @click="openModal(image)"/> 
      </div>

      

      
    </div>
    <Modal :showModal="modalVisible" :selectedImage="selectedImage" @close="closeModal" />
  </div>
</template>

<script>
import Modal from './imageDescription.vue'
export default {
 components: {
   Modal, 
 },
  data() {
    return {
      images: [],
      loading: true,
      accessToken: 'vRoyofjpkNCpCLrftTnbHkbYj5rL5O_FKw-D9jCpkXM',
      url: 'https://api.unsplash.com/photos/random',
      count: 8,
      modalVisible: false, 
      selectedImage: null, 
      searchKeyword: "",
    };
  },
  mounted() {
    this.fetchImages();
    console.log(this.images)
  },
  methods: {
   
   openModal(image) {
     this.modalVisible = true;
     this.selectedImage = image;
   },

   closeModal() {
     this.modalVisible = false;
     this.selectedImage = null;
   },
    fetchImages() {
      const promises = [];

      for (let i = 0; i < this.count; i++) {
        const promise = fetch(`${this.url}?client_id=${this.accessToken}`)
          .then(response => response.json())
          .then(json => {
            this.images.push(json);
          })
          .catch(error => {
            console.error(error);
          });

        promises.push(promise);
      }

      Promise.all(promises)
        .then(() => {
          this.loading = false;
        });
    },
  },
  computed: {
  filteredImages() {
    if (!this.searchKeyword) {
      return this.images; 
    }


    return this.images.filter((image) => {
      const altDescription = image.alt_description.toLowerCase();
      return altDescription.includes(this.searchKeyword.toLowerCase());
    });
  },
},

};
</script>

<style>
 .image_card{
   text-align: center;
   max-width: 200px;

 }

 .image_card p{
    margin-bottom: 10px;
 }

 h1{
   text-align: center;
 }


 .image_class-wrapper{
   margin: 0 auto;
   display: flex;
   justify-content: space-around;
   max-width: 880px;
   flex-wrap: wrap;
 }

 .image_card img{
   border-radius: 10px;
   cursor: pointer;
 }

 .search{
   padding: 5px;
   outline: none;
 }

 .search__block{
  text-align: center;
 }
</style>