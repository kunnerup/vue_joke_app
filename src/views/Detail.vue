<template>
<section id="detailview">
    <router-link to="/">
    <h2 class="backb">🢀TILBAGE</h2>
     </router-link>
      <h4>DETALJER OM JOKEN</h4>
        <p>Joken er indsendt af: {{post.uploadName}}</p>
        <br>
              <h3>{{post.name}}</h3>
                    <p class="category">Kategori: {{post.category}}</p>
      <p class="joke">{{post.description}}</p>
              <img :src="post.image" alt />
        </section>
</template>

<script>
export default {
  name: 'Detail',
  props: {
    post: Object
  }
}
</script>

<style>
.backb {
    margin: auto;
    width: 200px;
    color: white;
    background-color: #1989b2;
    padding-top: 10px;
    padding-bottom: 10px;
}

#detailview img {
max-width: 300px;
}
</style>
