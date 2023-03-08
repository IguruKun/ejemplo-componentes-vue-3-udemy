<script setup>
import { onMounted, ref } from "vue";
import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpiner from './components/LoadingSpiner.vue'

const posts =ref([]);

const postXpage = 3;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref('');

const cambiarFavorito = (tittle) => { 
  favorito.value=tittle;
 };

const next = () => { 
  inicio.value +=   postXpage;
  fin.value +=  postXpage;
 };

const prev = () => { 
  inicio.value -= postXpage;
  fin.value -= postXpage;
 };

 onMounted(async() => {
    try {
        const res = await fetch('https://jsonplaceholder.typicode.com/posts')
        posts.value = await res.json()
    } catch (error) {
        console.log(error);
    }finally{
        loading.value = false
    }
 })

//  fetch('https://jsonplaceholder.typicode.com/posts')
//  .then((res) => res.json())
//  .then((data) => {posts.value = data})
//  .catch(e => console.log(e))
//  .finally(() => {
//   setTimeout(() => {
//     loading.value = false
//   }, 1500)
//   });
</script>


<template>
  <LoadingSpiner v-if="loading"/>
  <div class="container" v-else>
    <h1>POSTS</h1>
    <h2>Mi post favorito: {{ favorito }}</h2>

    <PaginatePost 
        @prev="prev" 
        :inicio="inicio"  
        :fin="fin" 
        @next="next"
        :maxLength="posts.length"
        class="mb-2"
    />

    <BlogPost
        v-for="post in posts.slice(inicio, fin)" 
        :key="post.id"
        :title="post.title"
        :id="post.id" 
        :body="post.body" 
        @enviarFavorito="cambiarFavorito"      
        class="mb-2"
    ></BlogPost>
  </div>
</template>