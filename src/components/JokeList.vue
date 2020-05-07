<template>
<section>
     <div class="search-wrapper">
       <select v-model="selected">
           <option disabled value="">Vælg kategori</option>
  <option>Alle børnene</option>
  <option>Københavner-jokes</option>
  <option>Aarhusianer-jokes</option>
  <option>Far Jokes</option>
  <option>Banke-banke på</option>
  <option>Diverse</option>
       </select>
    <input type="text" v-model="search" placeholder="Søg efter jokes.."/>
  </div>
  <div class="grid-container">
    <article v-for="post in filteredPosts" :key="post.id">
      <img :src="post.image" alt />
      <div id="flexbox">
        <p v-on:click="deleteUser('{{post.id}}')">Slet joke</p>
        <p>Rediger joke</p></div>
      <h3>{{post.name}}</h3>
      <p>{{post.description}}</p>
      <p>{{post.id}}</p>
    </article>
  </div>
</section>
</template>

<script>
import { postRef } from '../firebase-db'
export default {
  data () {
    return {
      posts: [],
      search: '',
      selected: '',
      id: ''
    }
  },
  firestore: {
    posts: postRef
  },
  computed: {
    filteredPosts () {
      return this.posts.filter((post) => {
        return post.name.toLowerCase().match(this.search.toLowerCase())
      })
    },
    deleteJoke (id) {
      return this.id.delete
    }
  }
}
</script>

<style>
.grid-container {
  display: grid;
  grid-template-columns: 100%;
  padding: 10px;
}

@media (min-width: 600px) {
  .grid-container {
    grid-template-columns: 50% 50%;
  }
}

@media (min-width: 992px) {
  .grid-container {
    grid-template-columns: 33.33% 33.33% 33.33%;
  }
}

.grid-container > article {
  text-align: center;
  padding: 10px;
}

.grid-container > article img {
  max-width: 300px;
  height: 200px;
  width: 100%;
  object-fit:cover;
}

.grid-container > article h3 {
  font-weight: 200;
  margin: 0 0 1.5em;
}

button {
  border: none;
  padding: 12px 15px;
  font-size: 1em;
  color: white;
  background-color: #1989b2;
  width: 100%;
  max-width: 300px;
   text-align: center;
  cursor: pointer;
}
input, textarea, select {
  margin: 1em auto;
  width: 100%;
  max-width: 300px;
  padding: 12px 15px;
  box-sizing: border-box;
  display: block;
}

input[type="file"] {
    display: none;
}

#flexbox {
  display: flex;
  position:relative;
  background-color: white;
  opacity: 0.8;
  font-size: 0.7em;
  margin-top: -35px;
  justify-content: center;
}

#flexbox p {
  padding-left: 15px;
  padding-right: 15px;
}

</style>
