<template>
<section>
     <div class="search-wrapper">
       <select>
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
        <p v-on:click="deleteJoke(post.id)" :key="post.id">Slet joke</p>
        <p v-on:click="display = !display">Rediger joke</p>
        </div>
      <h3>{{post.name}}</h3>
      <p class="category">{{post.category}}</p>
      <p class="joke">{{post.description}}</p>
      <div class="updateInputs" v-bind:class="{display: !display}">
        <p v-on:click="display = !display">X</p>
        <h4>OPDATÉR JOKE</h4>
        <form>
  <h3>Joke-navn</h3>
      <input type="text" v-model="post.name" placeholder="Indtast jokens navn" required>
        <h3>Indtast din joke</h3>
<textarea type="text" v-model="post.description" placeholder="Indtast selve joken" required></textarea>
<h3>Vælg kategori</h3>
<select v-model="post.category">
  <option disabled>Vælg venligst en kategori</option>
  <option>Alle børnene</option>
  <option>Københavner-jokes</option>
  <option>Aarhusianer-jokes</option>
  <option>Far Jokes</option>
  <option>Banke-banke på</option>
  <option>Diverse</option>
</select>
        <h3>Vælg billede til din joke</h3>
      <input type="file" ref="fileInput" accept="image/*" v-on:change="previewImage">
      <button class="choose-image" type="button" v-on:click="triggerChooseImg">Vælg et nyt billede</button>

        <h3>Dit navn</h3>
<input type="text" v-model="post.uploadName" placeholder="Indtast dit navn" required>

      <div>
        <img :src="post.image" class="image-preview">
        <p>{{post.name}}</p>
      </div>
      <button type="button" v-on:click="updatePost(post.id, post.name, post.description, post.category, post.uploadName, post.image)">Opdatér Joke</button>
    </form>
        </div>
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
      id: '',
      display: true,
      name: '',
      description: '',
      category: '',
      uploadName: '',
      image: ''
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
    updatePost (id, name, description, category, uploadName, image) {
      postRef.doc(id).set({
        name,
        description,
        category,
        uploadName,
        image
      })
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
  max-width: 300px;
  margin: auto;
  font-size: 0.7em;
  margin-top: -35px;
  justify-content: space-around;
}

.details {
  background-color: #1989b2;
  padding: 15px;
  position: absolute;
}

.updateInputs {
  display: none;
}

.updateInputs.display {
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

</style>
