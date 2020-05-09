<template>
<section>
     <div class="search-wrapper">
       <select>
      <option value="" disabled selected>Vælg kategori</option>
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
               <router-link
          :key="post.id"
          :to="{name: 'detail', params: {post: post} }">
      <img :src="post.image" alt />
               </router-link>
      <div id="flexbox">
        <p v-on:click="deleteJoke(post.id)" :key="post.id">Slet joke</p>
        <router-link
          :to="{name: 'update', params: {post: post} }">
        <p>Rediger joke</p>
          </router-link>
        <div id="setflex" @click="incrementCounter">
       <p>❤ {{count}}</p>
        </div>
        </div>
         <router-link
          :key="post.id"
          :to="{name: 'detail', params: {post: post} }">
      <h3>{{post.name}}</h3>
      <p class="category">{{post.category}}</p>
      <p class="joke">{{post.description}}</p>
      </router-link>
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
      id: '',
      name: '',
      description: '',
      category: '',
      uploadName: '',
      image: '',
      search: '',
      count: 0
    }
  },
  firestore: {
    posts: postRef
  },
  methods: {
    deleteJoke (id) {
      postRef.doc(id).delete()
    },
    triggerChooseImg () {
      this.$refs.fileInput.click()
    },
    previewImage () {
      const imageFile = this.$refs.fileInput.files[0]
      const fileReader = new FileReader()
      fileReader.onload = (event) => {
        this.post.image = event.target.result
      }
      fileReader.readAsDataURL(imageFile)
    },
    incrementCounter: function () {
      this.count += 1
    }
  },
  computed: {
    filteredPosts () {
      return this.posts.filter((post) => {
        return post.name.toLowerCase().match(this.search.toLowerCase())
      })
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

.joke {
  max-width: 300px;
  margin: auto;
   margin-top: -15px;
   white-space: pre-wrap;
}

.category {
  padding-bottom: 10px;
  margin-top: -25px;
  font-size: 0.7em;
}

.grid-container > article img {
  max-width: 300px;
  height: 200px;
  width: 100%;
  object-fit:cover;
  opacity: 0.8;
}

.grid-container > article img:hover {
opacity: 1;
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
  border: 1px solid #1989b2;
}

.choose-image{
  border: 1px solid #1989b2;
}

input[type="file"] {
    display: none;
}

#flexbox {
  display: flex;
  position:relative;
  background-color: white;
  opacity: 0.8;
  max-width: 300px;
  margin: auto;
  font-size: 0.7em;
  margin-top: -35px;
  justify-content: space-around;
  cursor: pointer;
}

a {
  color: black;
  text-decoration: none;
}

.details {
  background-color: #1989b2;
  padding: 15px;
  position: absolute;
}
.updateInputs .active {
  display: block;
  position: absolute;
  top:0;
  left:0;
  bottom: 0;
 background-color: white;
 box-shadow: 4px 4px 10px black;
 width: 100%;
 height: auto;
overflow: hidden;
overflow-y: scroll;
padding-bottom: 10px;
}

#setflex {
 background-image: url(../assets/like.png);
 background-size: contain;
 background-repeat: no-repeat;
 height: 30px;
 width: 35px;
}

#setflex p {
  width: 20px;
  color: white;
  width: 100%;
  margin-top: 6px;
}

</style>
