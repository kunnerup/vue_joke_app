<template>
  <div class="Create">
    <h1>OPRET JOKE</h1>
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
      <button class="choose-image" type="button" v-on:click="triggerChooseImg">Vælg et billede</button>

        <h3>Dit navn</h3>
<input type="text" v-model="post.uploadName" placeholder="Indtast dit navn" required>

      <div>
        <img :src="post.image" class="image-preview">
      </div>
      <button type="button" v-on:click="createPost">Indsend joke</button>
    </form>

  </div>
</template>

<script>
import { postRef } from '../firebase-db'
export default {
  name: 'Opret',
  data () {
    return {
      post: {
        name: '',
        description: '',
        categori: '',
        uploadName: '',
        image: null
      }
    }
  },
  methods: {
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
    createPost () {
      postRef.add(this.post)
      this.$router.push('/')
    }
  }
}
</script>

<style>
form {
  padding: 2em 1em 2.5em;
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

button.choose-image {
  background-color: white;
  border: grey 1px solid;
  color: grey;
  text-align: left;
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

.image-preview {
  max-width: 300px;
  width: 100%;
  object-fit: scale-down;
  padding: 1em 0;
}
</style>
