<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <joke-list></joke-list>
  </div>
</template>

<script>
import JokeList from '../components/JokeList.vue'
export default {
  components: {
    JokeList
  }
}
</script>
