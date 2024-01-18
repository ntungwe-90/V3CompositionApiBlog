<template>
  <div class="home">
    <p>homePage</p>
    <div v-if="error">{{ error }}</div>
    <PostList :posts="posts" />
    <!-- <button @click="showPosts = !showPosts">Toggle posts</button>
    <button @click="posts.pop()">delete a post</button> -->
  </div>
</template>

<script>
import PostList from '../components/PostList.vue'
import { ref } from 'vue'

export default {
  name: 'HomeView',
  components: { PostList },
  setup() {
    const posts = ref([])
    const error = ref(null)

    const load = async () => {
      try {
        let data = await fetch('http://localhost:3000/posts')
        if (!data.ok) {
          throw Error('no data available')
        }
        posts.value = await data.json()
      }
      catch (err) {
        error.value = err.message
        console.log(error.value)
      }
    }
    load()
    return { posts }
  }

}
</script>

<style scoped>
.home {
  background: teal;
}
</style>