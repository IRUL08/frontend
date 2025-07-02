<template>
  <div>
    <h2>Tambah Post</h2>
    <form @submit.prevent="addPost">
      <input v-model="newPost.title" placeholder="Judul" required />
      <input v-model="newPost.body" placeholder="Konten" required />
      <button type="submit">Kirim</button>
    </form>

    <hr />

    <h2>Daftar Post</h2>
    <ul>
      <li v-for="post in posts" :key="post.id">
        <strong>{{ post.title }}</strong> - {{ post.body }}
        <button @click="deletePost(post.id)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PostApp',
  data() {
    return {
      posts: [],
      newPost: {
        title: '',
        body: ''
      }
    }
  },
  mounted() {
    this.loadPosts()
  },
  methods: {
    async loadPosts() {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
        this.posts = response.data
      } catch (error) {
        console.error('Gagal mengambil data:', error)
      }
    },
    async addPost() {
      try {
        const response = await axios.post('https://jsonplaceholder.typicode.com/posts', this.newPost)

        // Tambahkan ke tampilan langsung
        this.posts.unshift({
          id: Date.now(), // ID unik
          title: this.newPost.title,
          body: this.newPost.body
        })

        this.newPost.title = ''
        this.newPost.body = ''
      } catch (error) {
        console.error('Gagal menambahkan post:', error)
      }
    },
    deletePost(id) {
      // Hapus langsung dari tampilan
      this.posts = this.posts.filter(post => post.id !== id)
    }
  }
}
</script>
