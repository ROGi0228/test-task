<!-- Modal.vue -->
<template>
   <div class="modal" v-if="showModal">
     <div class="modal-content">
       <span class="close" @click="closeModal">&times;</span>
       <img :src="selectedImage.urls.full" :alt="selectedImage.alt_description" />
       <p>Username: {{ selectedImage.user.username }}</p>
       <p>Likes: {{ selectedImage.likes }}</p>
       <p>Downloads: {{ selectedImage.downloads }}</p>
       <p>descriprion: {{ selectedImage.alt_description }}</p>
     </div>
   </div>
 </template>
 
 <script>
 export default {
   props: {
     showModal: Boolean,
     selectedImage: Object,
   },
   methods: {
     closeModal() {
       this.$emit('close');
     },
   },
 };
 </script>
 
 <style>
 
 .modal {
   display: block; 
   position: fixed;
   z-index: 1;
   left: 0;
   top: 0;
   width: 100%;
   height: 100%;
   background-color: rgba(0, 0, 0, 0.8); 
 }
 
 .modal-content {
   position: absolute;
   top: 20%;
   left: 40%;
   color: white;
   max-width: 300px;
 }
 

 .close {
   color: white;
   font-size: 30px;
   font-weight: bold;
   position: absolute;
   top: 0;
   right: 15px;
   cursor: pointer;
 }
 
 .close:hover {
   color: #bbb;
 }

 .modal-content img{
  width: 300px;
  height: 300px;
  border-radius: 10px;

 }
 </style>
 